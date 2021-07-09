# 一、思路

- 首先，开始时选定一个结点，将该结点加入集合（此时集合只有一个元素），记录该结点到其他结点的路径。
- 找到在集合内找集合外**最近**的一个结点，使该结点加入集合，并更新集合内结点到其他结点的最短路径。
- 循环遍历n次，直到集合内没有结点完成。



代码思路：

- 参数定义：定义存储结点间路径二维数组A[MAX] [MAX]，最短路径dist[MAX]，前驱path[MAX]，是否在集合内isCheck[MAX]。
- 初始化：函数输入参数v，即最开始加入集合的节点初始化，配置v到其他结点的路径数组dist[]
- 遍历：寻找集合外离集合内最近的节点加入集合，并更新最短路径数组dist[]





# 二、代码

```cpp
#include <iostream>
using namespace std;
#include <queue>
#include <stack>
#include <algorithm>
#include <string>
#include <vector>
#include <list>


#define INF 32767
#define MAX 5
int dist[MAX];
int path[MAX];
int A[MAX][MAX];

void dijkstra(int v)//求得v到其他顶点的最短路径
{
    bool isCheck[MAX];//是否已经加入集合
    int n = MAX;
    /* 初始化顶点v到其他结点的路径和前驱 */
    for (int i = 0; i < n; i++)
    {
        isCheck[i] = false;
        dist[i] = A[v][i];//初始化和v连接的节点
        if (A[v][i] != INF)//没有连接的节点都是INF
        {
            path[i] = v;//前驱结点都是v
        }
        else
        {
            path[i] = -1;//否则为-1
        }
    }
    /************************************/
    isCheck[v] = true;
    dist[v] = 0;//自身到自身的路径自然为0
    /* 以下遍历n-1次，找到v到各节点的最短路径 */
    for (int i = 1; i < n; i++)
    {
        int u = 0, minLen = INF;
        /* 找到下一个加入集合的节点，在未入集合且可被选中的节点中找最短路径 */
        for (int j = 0; j < n; j++)
        {
            if (!isCheck[j] && dist[j] < minLen)
            {
                minLen = dist[j];//更新找到的最短路径
                u = j;//找到下标
            }
        }
        /************************************/
        isCheck[u] = true;
        /* 新加入结点后，修改到各节点的最短路径 */
        for (int j = 0; j < n; j++)
        {
            if (!isCheck[j] && A[u][j] != INF)
            {
                if (dist[u] + A[u][j] < dist[j])//dist[u]+A[u][j]即为v到j的路径，可以理解为在之前到u的基础上加上到其他结点的距离，并与之前dist[j]取最小
                {
                    dist[j] = dist[u] + A[u][j];//更新最短路径
                    path[j] = u;//修改前驱
                }
            }
        }
    }

}


int main()
{
    
    for (int i = 0; i < MAX; i++)
    {
        for (int j = 0; j < MAX; j++)
        {
            A[i][j] = INF;
        }
    }
    A[0][1] = 10;
    A[0][4] = 5;
    A[1][2] = 1;
    A[1][4] = 2;
    A[2][3] = 4;
    A[3][0] = 7;
    A[3][2] = 6;
    A[4][1] = 3;
    A[4][2] = 9;
    A[4][3] = 2;

    dijkstra(0);
    for (int i = 0; i < MAX; i++)
    {
        cout << dist[i] << endl;
    }
    
    
}
```





# 三、代码二

```cpp
#include <iostream>
using namespace std;
#include <queue>
#include <stack>
#include <algorithm>
#include <string>
#include <vector>
#include <list>


#define INF 32767
#define MAX 5
int dist[MAX];
int path[MAX];
int A[MAX][MAX];
bool isCheck[MAX];//是否已经加入集合

void dijkstra(int v)//求得v到其他顶点的最短路径
{
    
    int n = MAX;
    
    dist[v] = 0;
    path[v] = -1;
    
    /* 以下遍历n-1次，找到v到各节点的最短路径 */
    for (int i = 0; i < n; i++)
    {
        int u = -1, minLen = INF;
        /* 找到下一个加入集合的节点，在未入集合且可被选中的节点中找最短路径 */
        for (int j = 0; j < n; j++)
        {
            if (!isCheck[j] && dist[j] < minLen)
            {
                minLen = dist[j];//更新找到的最短路径
                u = j;//找到下标
            }
        }
        /************************************/
        isCheck[u] = true;
        /* 新加入结点后，修改到各节点的最短路径 */
        for (int j = 0; j < n; j++)
        {
            if (!isCheck[j] && A[u][j] != INF)
            {
                if (dist[u] + A[u][j] < dist[j])//dist[u]+A[u][j]即为v到j的路径，可以理解为在之前到u的基础上加上到其他结点的距离，并与之前dist[j]取最小
                {
                    dist[j] = dist[u] + A[u][j];//更新最短路径
                    path[j] = u;//修改前驱
                }
            }
        }
    }

}


int main()
{
    
    fill(A[0], A[0] + MAX * MAX, INF);
    fill(dist, dist + MAX, INF);
    A[0][1] = 10;
    A[0][4] = 5;
    A[1][2] = 1;
    A[1][4] = 2;
    A[2][3] = 4;
    A[3][0] = 7;
    A[3][2] = 6;
    A[4][1] = 3;
    A[4][2] = 9;
    A[4][3] = 2;

    dijkstra(0);
    for (int i = 0; i < MAX; i++)
    {
        cout << dist[i] << " " << path[i] << endl;
    }
    
    
}
```

