# 最短路笔记

 最短路分为两种，单源最短路和多源最短路
 单源最短路：分两种情况：
 所有边权为正，可以使用dijkstra算法，朴素版时间复杂度为O(n^2)，n为点数
 堆优化版时间复杂度为O(mlogn)，m为边数
 存在负权边，可以使用spfa算法，时间复杂度为O(km)，k为常数
 还有Bellman-Ford算法，时间复杂度为O(nm)
 多源最短路：可以使用floyd算法，时间复杂度为O(n^3)
 同时最短路算法还可以求解负环问题,差分约束.
 
