---
layout: post
title: LeedCode 37.Sudoku Solver
date: 2019-03-07 12:00:00
tags: [LeetCode, 刷题]
categorise: 刷题
top: 0

---

LeedCode 37.Sudoku Solver
====


# 题目描述
<!-- more -->
就是数独游戏
# 解法思路
把所有空位置找出来,并找到该位置可以填的数字的集合.针对每个空位置,把其集合中的数字依次填进去验证(这里采用递归):
    * 如果返回flase,就把该位置重新设为空,验证集合中的下一个数字
    * 直到递归完成,返回true
# 解法分析
* 方法1: 采用set\<char\>存储数字集合,Runtime: 248ms(unordered\_set也差不多)
* 方法2: 采用vector\<int\>表示数字集合,Runtime: 36ms
* 方法3: 采用int,位运算表示数字集合,Runtime: 12ms
* 可见,能用简单数据结构实现还是最好的

# 代码
* 方法1:
```c
class Solution {
public:
    void solveSudoku(vector<vector<char>>& board) {
        solve(board);
    }
    bool solve(vector<vector<char>>& board){
        for(int i = 0; i < 9; ++i){
            for(int j = 0; j < 9; ++j){
                if(board[i][j] == '.'){
                    set<char> tmp;
                    for(int k = 0; k < 9; ++k){
                        if(board[i][k] != '.')
                            tmp.insert(board[i][k]);
                        if(board[k][j] != '.')
                            tmp.insert(board[k][j]);
                    }
                    for(int k = 0; k < 3; ++k){
                        for(int m = 0; m < 3; ++m){
                            int ii = i/3*3+k;
                            int jj = j/3*3+m;
                            if(board[ii][jj] != '.'){
                                tmp.insert(board[ii][jj]);
                            }
                        }
                    }
                    
                    for(char c = '1'; c <= '9'; ++c){
                        if(tmp.find(c) == tmp.end()){
                            board[i][j] = c;
                            if(solve(board))
                                return true;
                            else
                                board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
};
```
* 方法2:
```c
class Solution {
public:
    void solveSudoku(vector<vector<char>>& board) {
        solve(board);
    }
    bool solve(vector<vector<char>>& board){
        for(int i = 0; i < 9; ++i){
            for(int j = 0; j < 9; ++j){
                if(board[i][j] == '.'){
                    vector<int> tmp(9,0);
                    for(int k = 0; k < 9; ++k){
                        if(board[i][k] != '.')
                            tmp[board[i][k]-'0'-1] = 1;
                        if(board[k][j] != '.')
                            tmp[board[k][j]-'0'-1] = 1;
                        if(board[i/3*3+k/3][j/3*3+k%3] != '.')
                            tmp[board[i/3*3+k/3][j/3*3+k%3]-'0'-1] = 1;
                    }
                    
                    for(int k = 0; k < 9; ++k){
                        if(!tmp[k]){
                            board[i][j] = k + '0' + 1;
                            if(solve(board))
                                return true;
                            else
                                board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
};
```
* 方法3:
```c
lass Solution {
public:
    void solveSudoku(vector<vector<char>>& board) {
        solve(board);
    }
    bool solve(vector<vector<char>>& board){
        for(int i = 0; i < 9; ++i){
            for(int j = 0; j < 9; ++j){
                if(board[i][j] == '.'){
                    int row = 0x1ff;
                    int col = 0x1ff;
                    int squ = 0x1ff;
                    for(int k = 0; k < 9; ++k){
                        if(board[i][k] != '.')
                            row ^= 1 << (board[i][k] - 49);
                        if(board[k][j] != '.')
                            col ^= 1 << (board[k][j] - 49);
                        if(board[i/3*3+k/3][j/3*3+k%3] != '.')
                            squ ^= 1 << (board[i/3*3 + k/3][j/3*3 + k%3] - 49);
                    }
                    int flag = row & col & squ;
                    
                    for(int k = 0; k < 9; ++k,flag>>=1){
                        if(flag%2){
                            board[i][j] = k + '0' + 1;
                            if(solve(board))
                                return true;
                            else
                                board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
};
```
