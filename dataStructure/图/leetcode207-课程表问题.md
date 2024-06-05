[题目](https://leetcode.cn/problems/course-schedule/description/)

你这个学期必须选修 `numCourses` 门课程,记为 `0 到 numCourses - 1` . 

在选修某些课程之前需要一些先修课程.  先修课程按数组 `prerequisites` 给出,其中 `prerequisites[i] = [ai, bi]` ,表示如果要学习课程 ai 则 必须 先学习课程  bi . 

例如,先修课程对 `[0, 1] `表示: 想要学习课程 0 ,你需要先完成课程 1 . 
请你判断是否可能完成所有课程的学习?如果可以,返回 true ; 否则,返回 false . 

示例 1: 
输入: numCourses = 2, prerequisites = [[1,0]]
输出: true
解释: 总共有 2 门课程. 学习课程 1 之前,你需要完成课程 0 . 这是可能的. 

示例 2: 
输入: numCourses = 2, prerequisites = [[1,0],[0,1]]
输出: false
解释: 总共有 2 门课程. 学习课程 1 之前,你需要先完成​课程 0 ; 并且学习课程 0 之前,你还应先完成课程 1 . 这是不可能的. 


## 思路
这是一道经典的拓扑排序问题,所谓拓扑排序

比如吃自助火锅,有一套约定俗成的流程,首先先打开包装,然后放入粉、佐料、菜,然后在加水,最后盖上盖子

这种关系通常使用有向图来表示,如果这套流程能够成功的帮助你最后吃到火锅(无环),那这种依赖顺序就是拓扑排序,即拓扑排序是针对有向无环图的

使用 `邻接表` 来表示有向图中各个节点的依赖关系,同时维护一个`入度表`,则`入度表中入度为 0 `的节点所表示的课程是可以立即开始学习的

```js
/**
 * @param {number} numCourses
 * @param {number[][]} prerequisites
 * @return {boolean}
 */
var canFinish = function(numCourses, prerequisites) {
  //  没有先决条件,则都可以完成
  if (prerequisites.length === 0) {
    return true
  }
  // 维护入度表
  let inDegree = Array(numCourses).fill(0)
  // 维护邻接表
  let adj = new Map()

  for (let [later, first] of prerequisites) {
    //  统计入度数量
    inDegree[later]++
    //  邻接表不存在,则创建
    if (!adj.has(first)) {
      adj.set(first, [])
    }
    //  加入某项的拓扑数组中
    let vEdge = adj.get(first)
    vEdge.push(later)
  }

  let queue = []
  // 首先加入入度为 0 的结点, 即不需要先决条件的课程
  for (let i=0; i<numCourses; i++) {
    if (inDegree[i] === 0) {
      queue.push(i)
    }
  }

  while(queue.length>0) {
    // 出队一门课程
    let v = queue.shift()
    numCourses--

    if (!adj.has(v)) continue
    //  adj.get(v) 这个是一个数组,所以w 就是数组里的元素
    for(let w of adj.get(v)) {
      inDegree[w]--
      if (inDegree[w] === 0) {
        queue.push(w)
      }
    }
  }

  return numCourses === 0

};

时间复杂度: O(V+E),遍历一个图需要访问所有节点和边
空间复杂度: O(V+E),用于存储临接表等
```