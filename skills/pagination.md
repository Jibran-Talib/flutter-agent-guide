# Pagination Skill

## ApiResponse Model

The core paginated response wrapper (`core/api_service/api_response.dart`):

```dart
class ApiResponse<T> {
  final List<T> data;
  final int total;
  final int page;
  final int limit;
  final bool hasNextPage;

  const ApiResponse({
    required this.data,
    required this.total,
    required this.page,
    required this.limit,
    required this.hasNextPage,
  });

  factory ApiResponse.fromJson(
    Map<String, dynamic> json,
    T Function(Map<String, dynamic>) fromJson,
  ) {
    return ApiResponse(
      data: (json['data'] as List).map((e) => fromJson(e as Map<String, dynamic>)).toList(),
      total: json['total'] ?? 0,
      page: json['page'] ?? 1,
      limit: json['limit'] ?? 10,
      hasNextPage: json['hasNextPage'] ?? false,
    );
  }
}
```

---

## Pagination State Pattern

```dart
enum FeedAction { load, loadMore, refresh }

class FeedState extends Equatable {
  final FeedStatus status;
  final FeedAction? action;
  final List<PostEntity> posts;
  final int page;
  final bool hasNextPage;
  final String? message;

  const FeedState({
    this.status = FeedStatus.initial,
    this.action,
    this.posts = const [],
    this.page = 1,
    this.hasNextPage = true,
    this.message,
  });

  bool isLoadingOf(FeedAction a) => status == FeedStatus.loading && action == a;

  FeedState copyWith({...}) => FeedState(...);

  @override
  List<Object?> get props => [status, action, posts, page, hasNextPage, message];
}

enum FeedStatus { initial, loading, success, failure }
```

---

## Cubit with Load More

```dart
class FeedCubit extends Cubit<FeedState> {
  final GetPostsUseCase _getPosts;

  FeedCubit({required GetPostsUseCase getPosts})
      : _getPosts = getPosts,
        super(const FeedState());

  Future<void> loadPosts({bool refresh = false}) async {
    if (refresh) {
      emit(state.copyWith(
        status: FeedStatus.loading,
        action: FeedAction.refresh,
        page: 1,
        hasNextPage: true,
        posts: [],
      ));
    } else {
      emit(state.copyWith(
        status: FeedStatus.loading,
        action: FeedAction.load,
      ));
    }

    final result = await _getPosts.call(GetPostsParams(page: 1, limit: 10));

    result.fold(
      (failure) => emit(state.copyWith(
        status: FeedStatus.failure,
        message: failure.message,
      )),
      (response) => emit(state.copyWith(
        status: FeedStatus.success,
        posts: response.data,
        page: response.page,
        hasNextPage: response.hasNextPage,
      )),
    );
  }

  Future<void> loadMore() async {
    if (!state.hasNextPage || state.isLoadingOf(FeedAction.loadMore)) return;

    emit(state.copyWith(status: FeedStatus.loading, action: FeedAction.loadMore));

    final result = await _getPosts.call(
      GetPostsParams(page: state.page + 1, limit: 10),
    );

    result.fold(
      (failure) => emit(state.copyWith(
        status: FeedStatus.failure,
        message: failure.message,
      )),
      (response) => emit(state.copyWith(
        status: FeedStatus.success,
        posts: [...state.posts, ...response.data],
        page: response.page,
        hasNextPage: response.hasNextPage,
      )),
    );
  }
}
```

---

## UI with Scroll Controller

```dart
class FeedScreen extends StatefulWidget { ... }

class _FeedScreenState extends State<FeedScreen> {
  final _scrollController = ScrollController();

  @override
  void initState() {
    super.initState();
    context.read<FeedCubit>().loadPosts();
    _scrollController.addListener(_onScroll);
  }

  void _onScroll() {
    if (_scrollController.position.pixels >=
        _scrollController.position.maxScrollExtent - 300) {
      context.read<FeedCubit>().loadMore();
    }
  }

  @override
  void dispose() {
    _scrollController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return BlocBuilder<FeedCubit, FeedState>(
      builder: (context, state) {
        if (state.isLoadingOf(FeedAction.load)) return const AppLoader();

        return RefreshIndicator(
          onRefresh: () => context.read<FeedCubit>().loadPosts(refresh: true),
          child: ListView.builder(
            controller: _scrollController,
            itemCount: state.posts.length + (state.hasNextPage ? 1 : 0),
            itemBuilder: (context, index) {
              if (index == state.posts.length) {
                return const Center(child: AppLoader());  // load-more spinner
              }
              return PostCard(post: state.posts[index]);
            },
          ),
        );
      },
    );
  }
}
```
