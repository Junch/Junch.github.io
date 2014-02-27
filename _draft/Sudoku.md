和数独的第一次接触

在leetcode上随便选择一道题目叫做Valid, Sudoku,后来才知道是数独。

这个题目我估计可以用回溯法解决，但看着比N-Queens问题复杂。

N-Queens的Stack只需要记录每个Queen的坐标，如果这个Queen遍历完了，下一个Queen在同一行中往后选择，继续扩展。N-Queens的基本框架如下所示。

```
public ArrayList<String[]> solveNQueens(int n) {
    ArrayList<String[]> arr = new ArrayList<String[]>();

    Stack<Node> S = new Stack<Node>();
    S.push(new Node(0, 0));

    // S holds the valid positions in previous rows
    while(!S.empty()){

        if (getASolution(S, n))
            addSolution(S, arr);
        else if(GetQueenOnNextRow(S, n))
            continue;

        ChangeQueenToExpand(S, n);
    }

    return arr;
}

```

Sudoku的难度就在于这个回溯，也就是上图中的函数ChangeQueenToExpand(S, n)。
1. 关于单个cell的回溯
  ［56x847xxx］对于这一列，如何回溯呢？假设第一个x现在的值为1，如果回溯到它，下个取值应该是选择2。其取值顺序为[1,2,3,9]
   如果第一个选择了1，那么第二个x就不能选择1了，只能选择从2开始，如果从2开始到，那么其取值应该是
   [2,3,9,1]这样一个顺序。回溯如何控制呢？
   
   x1 - 1,2,3,9
   x2 - 2,3,9
   x3 - 3,9
   x4 - 9
   
   如果最后一个数x4是9了，从这个点开始怎么回溯？x3取值改为9，x4取值改为3,然后继续回溯...
   对于单个cell而言，不容易控制回溯的起点或者终点。
   
   应该将1行的状态作为一个Stack的节点。
   

2. 关于拓展
   一行一行的扩展可行吗？每次试探1个值可以从行，列，Box三个方面来进行验证。
    
   每次试探的不是某个cell的取值，应该是这一行的所有cell的某个取值。然后验证这一行的取值可行否？也就是没有冲突，如果没有冲突，那么继续。
   
 
经过以上分析发现首先要解决一个问题，对于每个序列，如果计算下一个序列，比如从1,2,3 -> 1,3,2。这个问题刚好是LeetCode中的一道题目。 

1,2,5,4,3 变成 1,3,2,4,5
1. 找到拐点2,5
2. 将最后一个元素3提前到2现在到位置
3. 将2后移一个位置，变成了1,3,2,5,4
4. 将5,4直接倒序

4,5,3 变成 5,3,4
1, 首先是从5,3中取最小的大于4的元素, 那这个元素就是5
2, 剩下的元素,4,3要排序成3,4


