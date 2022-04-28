# <font color="#C0C4CC">leetcode note

***
  
# 2020-4-28     网格dfs-岛屿问题的通用解法
  https://leetcode-cn.com/problems/number-of-islands/solution/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-/
  
  网格化可以看做二叉树往四个方向遍历，结束条件是否越界
  
  463.岛屿的周长（easy）
  
  岛屿周长dfs 函数因为「坐标 (i, j) 超出网格范围」返回的时候，经过的边+1；而当函数因为「当前格子是海洋格子」返回的时候，经过的边+1。
  
  ```c++
  
    int islandPerimeter(vector<vector<int>>& grid) {
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==1){
                    return dfs(grid,i,j);
                }
            }
        }
        return 0;
    }

    int dfs(vector<vector<int>>& grid,int i,int j){
        if(!isArea(grid,i,j)){
            return 1;
        }
        if(grid[i][j]==0){
            return 1;
        }
        if(grid[i][j]!=1){
            return 0;
        }


        grid[i][j]=2;

        return dfs(grid,i-1,j)+dfs(grid,i+1,j)+dfs(grid,i,j-1)+dfs(grid,i,j+1);
    }

    bool isArea(vector<vector<int>>& grid,int i,int j){
        return 0<=i && i<grid.size() && 0<=j && j<grid[0].size();
    }

  ```
  
  200.岛屿数量(medium)
  
  避免重复遍历：将走过的格子置为2
  
  ```c++
    int numIslands(vector<vector<char>>& grid) {
        int count=0;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]=='1'){
                    dfs(grid,i,j);
                    count++;
                }
            }
        }
        return count;
    }

    void dfs(vector<vector<char>>& grid,int r,int c){
        if(!isinArea(grid,r,c)){
            return;
        }

        if(grid[r][c]!='1'){
            return;
        }
        grid[r][c]='2';

        dfs(grid,r-1,c);
        dfs(grid,r+1,c);
        dfs(grid,r,c-1);
        dfs(grid,r,c+1);

    }

    bool isinArea(vector<vector<char>>& grid,int r,int c){
        return 0<=r && r<grid.size() && 0<=c && c<grid[0].size();
    }
  
  ```c++
  
  
  695.岛屿的最大面积(medium)
  
  每遍历到一个格子，就把面积加一
  
  ```c++
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int res=0;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==1){
                    int a=dfs(grid,i,j);
                    res=a>res?a:res;
                }
            }
        }
        return res;
    }

    int dfs(vector<vector<int>>& grid,int r,int c){
        if(!isArea(grid,r,c)){
            return 0;
        }
        if(grid[r][c]!=1){
            return 0;
        }
        grid[r][c]=2;
        return 1+dfs(grid,r-1,c)+dfs(grid,r+1,c)+dfs(grid,r,c-1)+dfs(grid,r,c+1);
    }


    bool isArea(vector<vector<int>>& grid,int r,int c){
        return 0<=r && r<grid.size() && 0<=c && c<grid[0].size();
    }
  
  ```
  
  827.最大人工岛(hard)-待补
