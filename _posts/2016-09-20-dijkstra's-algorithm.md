---
title:      Dijkstra 算法
summary:    Dijkstra 算法JS实现、最短路径问题
categories: JavaScript Algorithm
tags:       JavaScript Algorithm 算法 Dijkstra 
---

## 复杂度

时间复杂度O(n<sup>2</sup>),空间复杂度（邻接矩阵）O(n<sup>2</sup>)


## JS代码实现

```javascript
/**
 * Dijkstra算法
 * @param matrix 邻接矩阵
 * @param v0 起点
 * @returns {Array}
 */
function shortestPathDijkstra(matrix, v0) {
    // D[i] 保存v0到顶点vi的最短路径值
    // final[i] = true 表示节点vi属于S，即已经求得v到vi的最短路径
    // p[v][w] = true 表示v0到v的最短路径经过w
    var D = [],
        final = [],
        P = [];

    var n = matrix.length,
        v, w;

    // 初始化
    for (v = 0; v < n; v++) {
        P[v] = [];
        final[v] = false;
        // 初始化最短路径值
        D[v] = matrix[v0][v];
        for (w = 0; w < n; w++) {
            P[v][w] = false; // 设置空路径
        }
        // 初始化最短路径
        if (D[v] < Infinity) {
            P[v][v0] = true;
            P[v][v] = true;
        }
    }

    // 将v0加入S中
    D[v0] = 0;
    final[v0] = true;
    // 遍历n-1次，每次将新的顶点加入到S中
    for (var i = 1; i < n; i++) {
        var min = Infinity;
        // 寻找v-S中，与v0距离最近的顶点
        for (w = 0; w < n; w++) {
            if (!final[w] && D[w] < min) {
                v = w;
                min = D[w];
            }
        }

        // 将v放入S中
        final[v] = true;
        for (w = 0; w < n; w++) {
            // 如果不在S集中，并且v0->v->w的路径长度小于原值，则更新最短路径
            if (!final[w] && (min + matrix[v][w] < D[w])) {
                // 更新路径值
                D[w] = min + matrix[v][w];
                // 更新路径
                for (var j = 0; j < n; j++) {
                    P[w][j] = P[v][j];
                }
                P[w][w] = true;
            }
        }
    }
    
    return D;
}
```

## 测试用例

如下图所示

![Dijkstra算法测试用例](/img/dijkstra-example.jpg)

测试结果

```javascript
// 测试用例
var matrix = [];
for (var i = 0; i < 7; i++) {
    matrix[i] = [];
    for (var j = 0; j < 7; j++) {
        matrix[i][j] = Infinity;
    }
}

// 有向图
matrix[0][1] = 4;
matrix[0][2] = 10;
matrix[1][4] = 21;
matrix[2][3] = 5;
matrix[2][5] = 8;
matrix[3][4] = 5;
matrix[4][5] = 12;
matrix[4][6] = 4;

shortestPathDijkstra(matrix, 0); // [0, 4, 10, 15, 20, 18, 24]

// 无向图
for (var i = 0; i < 7; i++) {
    for (var j = 0; j < 7; j++) {
        if (matrix[i][j] < Infinity) {
            matrix[j][i] = matrix[i][j];
        }
    }
}
shortestPathDijkstra(matrix, 6); // [24, 25, 14, 9, 4, 16, 0]
```


## 参考目录

- [Dijkstra's algorithm再理解](http://shmilyaw-hotmail-com.iteye.com/blog/2316491)
- [算法学习 - Dijkstra's Algorithm](http://blog.csdn.net/stanfordzhang/article/details/6626584)
- [迪杰斯特拉最短路径算法 严蔚敏C++实现](http://blog.csdn.net/ariessurfer/article/details/10554581)​

  ​