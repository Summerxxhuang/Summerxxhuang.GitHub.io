# <font color="#C0C4CC">leetcode note

***
  
# 2022-4-28     网格dfs-岛屿问题的通用解法
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
  
  ```
  
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
  
  
# 2022-4-29     二叉搜索树、avl树
  
  二叉搜索树与中序遍历高度相关；avl（高度平衡）树首先是一颗二叉搜索树，其次每个节点的左右子树高度差不超过1，左旋右旋？（待补）
  
  108. 将有序数组转换为二叉搜索树：不断取中点二分
  
  ```c++
  TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums,0,nums.size()-1);
    }

    TreeNode* helper(vector<int>& nums,int left,int right){
        if(left>right){
            return nullptr;
        }
        int mid=(left+right)/2;
        TreeNode* root=new TreeNode(nums[mid]);
        root->left=helper(nums,left,mid-1);
        root->right=helper(nums,mid+1,right);
        return root;
    }
  ```
  
  1382. 将二叉搜索树变平衡，和上边一题思路一样，先把二叉搜索树转变成数组，再转avl树
  
  ```c++
  vector<int> nums;
    TreeNode* balanceBST(TreeNode* root) {
        inOrder(root);
        return helper(nums,0,nums.size()-1);
    }

    void inOrder(TreeNode* root){
        if(root==nullptr){
            return;
        }
        inOrder(root->left);
        nums.push_back(root->val);
        inOrder(root->right);
    }

    TreeNode* helper(vector<int>& nums,int left,int right){
        if(left>right){
            return nullptr;
        }
        int mid=(left+right)/2;
        TreeNode* root=new TreeNode(nums[mid]);
        root->left=helper(nums,left,mid-1);
        root->right=helper(nums,mid+1,right);
        return root;
    }
  
  ```
  
  99.恢复二叉搜索树
  
  方法一：先把二叉树中序遍历拉成有序数组，找出两处满足ai>ai+1的地方，记为x和y，交换俩节点（法一官解交换的时候需要count没看懂），时间O(N)，空间O(N)
  
  方法二：在中序遍历时直接记录下俩异常结点，t1的值会比遍历到的后面一个节点值大，t2的值会比遍历到的前面一个节点值小，时间O(N)，空间O(H)（递归时隐含的栈操作，H为树高度）
  
  方法三：Moris中序遍历（待补）
  
  需要参数：

  一个前序指针pre指向前一个节点
  
  一个记录指针t1记录需要交换的第一个节点
  
  一个记录指针t2，在t1确定后记录需要交换的第二个节点
  
  ！！！无语了，TreeNode初始化只能这么写，写成TreeNode* t1,t2=nullptr;不行
  
  写成TreeNode* t1=nullptr,t2=nullptr;不行
  
  写TreeNode* t1=new TreeNode(-1)也不行（会将初始值置-1影响后面结果）
  
  ```c++
    TreeNode* t1=nullptr;
    TreeNode* t2=nullptr;
    TreeNode* pre=nullptr;
    void recoverTree(TreeNode* root) {
        inOrder(root);
        swap(t1->val,t2->val);
    }

    void inOrder(TreeNode* root){
        if(root==nullptr){
            return;
        }
        inOrder(root->left);
        if(pre!=nullptr && root->val < pre->val){
            if(t1==nullptr){
                t1=pre;
            }
            t2=root;
        }
        pre=root;
        inOrder(root->right);
    }
  ```
  
# 2022-5-9  521遗留问题
  
  34.在排序数组中查找元素的第一和最后一个位置
  
  二分法先找target的某个值，再滑动双指针找边界。Watch out边界出界条件！！
  
  ```c++
      vector<int> searchRange(vector<int>& nums, int target) {
        int index=binarySearch(0,nums.size()-1,nums,target);
        if (index==-1){
            return {-1,-1};
        }
        int left=index,right=index;
        //先判断越界再移动指针
        while(left>0 && nums[left-1]==nums[index]){
            --left;
        }
        while(right<nums.size()-1 && nums[right+1]==nums[index]){
            ++right;
        }
        return {left,right};
    }

    int binarySearch(int left,int right,vector<int>& nums,int target){
        if(left>right){
            return -1;
        }
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target){
                return mid;
            }else if(nums[mid]>target){
                right=mid-1;
            }else{
                left=mid+1;
            }
        }
        return -1;
    }
  ```
  
  79.单词搜索(回溯法
  
  类似岛屿问题，但判断出界问题时用&&要判断多个条件会导致超时？改用"或"一个不满足就叉出去了。也无法先试探后判断，四个方向不循环会导致代码量增大
  
  ```c++
      bool exist(vector<vector<char>>& board, string word) {
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[i].size();j++){
                if(dfs(board,word,i,j,0)) return true;
            }
        }
        return false;
    }
    vector<int> dx={-1,0,1,0},dy={0,-1,0,1};
    bool dfs(vector<vector<char>>& board,string word,int x,int y,int u){
        if(board[x][y]!=word[u]) return false;
        if(u==word.size()-1) return true;
        char t=board[x][y];
        board[x][y]='.';

        for(int i=0;i<4;i++){
            int a=x+dx[i],b=y+dy[i];
            if(a<0 || a>=board.size() || b<0 || b>=board[0].size() || board[a][b]=='.'){
                continue;
            }
            if(dfs(board,word,a,b,u+1)) return true;
        }
        board[x][y]=t;
        return false;
    }
  ```
  
  106.中序和后序构造二叉树
  
  从后续的最后获取root，查找到哈希表中对应中序的index，再根据index划分左右区间，先右后左恢复二叉树！
  
  ```c++
  int postIndex;
  unordered_map<int,int> indexMap;
  TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
      postIndex=(int)postorder.size()-1;
      //indexMap[i]=inorder[i]行不通
      for(int i=0;i<inorder.size();i++){
          indexMap[inorder[i]]=i;
      }
      return helper(0,(int)inorder.size()-1,inorder,postorder);
  }

  TreeNode* helper(int left,int right,vector<int>& inorder,vector<int>& postorder){
      if(left>right){
          return nullptr;
      }
      int rootVal=postorder[postIndex];
      TreeNode* root=new TreeNode(rootVal);
      int index=indexMap[rootVal];
      postIndex--;
      //注意这里有需要先创建右子树，再创建左子树的依赖关系。
      root->right=helper(index+1,right,inorder,postorder);
      root->left=helper(left,index-1,inorder,postorder);
      return root;
  }
  
  ```
  
  # 2022-5-10  二分法
  
  704.二分查找 
  
  ```c++
    int search(vector<int>& nums, int target) {
        if(nums.size()==0)return -1;
        return binarysearch(nums,target,0,nums.size()-1);
    }

    int binarysearch(vector<int>& nums,int target,int left,int right){
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]>target){
                right=mid-1;
            }else if(nums[mid]<target){
                left=mid+1;
            }else{
                return mid;
            }
        }
        return -1;
    }
  ```
                                        
  374.猜数字大小
                                        
  ```c++                                   
  int guessNumber(int n) {
        int left=1,right=n;
        while(left<right){
            int mid=left+(right-left)/2;
            int bigorsmall=guess(mid);
            if(bigorsmall==0){
                return mid;
            }else if(bigorsmall>0){
                left=mid+1;
            }else{
                right=mid-1;
            }
            
        }
        return left;
    }
  ```
  
  33.搜索旋转排序数组 
  
  ```c++
    int search(vector<int>& nums, int target) {
        if(nums.size()==0) return -1;
        if(nums.size()==1 && target!=nums[0]) return -1;
        int left=0,right=nums.size()-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target){return mid;}
            //注意等号位置
            if(nums[left]<=nums[mid]){
                if(nums[left]<=target && target<nums[mid]){
                    right=mid-1;
                }else{
                    left=mid+1;
                }
            }else{
                if(nums[mid]<target && target<=nums[right]){
                    left=mid+1;
                }else{
                    right=mid-1;
                }
            }
        }
        return -1;
    }
  ```
  
  81.搜索旋转排序数组II
  
  ```c++
  bool search(vector<int>& nums, int target) {
        if(nums.size()==0) return 0;
        if(nums.size()==1) return nums[0]==target?1:0;
        int left=0,right=nums.size()-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[mid]==target) return 1;
            if(nums[left]==nums[mid]){
                left++;
            }else if(nums[left]<nums[mid]){
                if(nums[left]<=target && target<=nums[mid]){
                    right=mid;
                }else{
                    left=mid+1;
                }
            }else{
                if(nums[mid]<=target && target<=nums[right]){
                    left=mid+1;
                }else{
                    right=mid;
                }
            }
        }
        return 0;
    }
  ```
  
  
  # 2022-5-11 二分法(2) 旋转数组
  
  189.轮转数组
  
  三次反转，第一次反转全部，第二次反转[0,k]，第三次反转[k,n]
  
  ```c++
    void rotate(vector<int>& nums, int k) {
      k=k % nums.size();
      reverse(nums.begin(),nums.end());
      reverse(nums.begin(),nums.begin()+k);
      reverse(nums.begin()+k,nums.end());
  }
  ```
  
  153.寻找旋转排序数组中的最小值
  
  ```c++
    int findMin(vector<int>& nums) {
      if(nums.size()==1) return nums[0];
      int left=0,right=nums.size()-1;
      int minNum=INT_MAX;
      while(left<=right){
          int mid=left+(right-left)/2;
          if(nums[left]<=nums[mid]){//左边有序
              minNum=min(minNum,nums[left]);
              left=mid+1;
          }else{//右边有序
              minNum=min(minNum,nums[mid+1]);
              right=mid;
          }
      }
      return minNum;
  }
  ```
  
  154.寻找旋转排序数组中的最小值2
  
  ```c++
    int findMin(vector<int>& nums) {
      int left=0,right=nums.size()-1;
      int minNum=nums[0];
      while(left<=right){
          int mid=left+(right-left)/2;
          if(nums[left]==nums[mid]){
              minNum=min(minNum,nums[left]);
              left++;
          }else if(nums[left]<nums[mid]){//左边有序
              minNum=min(minNum,nums[left]);
              left=mid+1;
          }else{//右边有序
              minNum=min(minNum,nums[mid]);
              right=mid;
          }
      }
      return minNum;
  }
  ```
  
  240. 搜索二维矩阵 II
  
  法一：一行一行的进行二分查找即可。

  此外，结合有序的性质，一些情况可以提前结束。

  比如某一行的第一个元素大于了 target ，当前行和后边的所有行都不用考虑了，直接返回 false。

  某一行的最后一个元素小于了 target ，当前行就不用考虑了，换下一行。

  ```c++
  
  //这版尚未跑通，没想出来vector<vector<int>> matrix[i]怎么传参进去
  bool searchMatrix(vector<vector<int>>& matrix, int target) {
        for(int i=0;i<matrix.size();i++){
            if(matrix[i][0]>target){
                break;
            }
            if(matrix[i][matrix[0].size()-1]<target){
                continue;
            }
            if(binarySearch(matrix[i],target)==1) return true;
        }
        return false;
    }

    bool binarySearch(vector<int>& nums,int target){
        int left=0,right=nums[].size()-1;
        while(left<=right){
            int mid=left+(right-left)/2;
            if(nums[][mid]==target){
                return true;
            }else if(nums[][mid]<target){
                left=mid+1;
            }else{
                right=mid;
            }
        }
        return false;
    }
  
  
  ```
  
  法二：从右上角出发开始遍历。向左数字会变小，向下数字会变大，有点和二分查找树相似。二分查找树的话，是向左数字变小，向右数字变大。

  所以我们可以把 target 和当前值比较。

  如果 target 的值大于当前值，那么就向下走。

  如果 target 的值小于当前值，那么就向左走。

  如果相等的话，直接返回 true 。

  ```c++
  
  bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0 || matrix[0].size()==0){
            return false;
        }
        int row=0,colomn=matrix[0].size()-1;
        while(colomn>=0 && row<matrix.size()){
            int temp=matrix[row][colomn];
            if(temp==target){
                return true;
            }else if(temp>target){
                colomn--;
            }else{
                row++;
            }
        }
        return false;
    }
  
  ```
  
  法三：我愿称之为正统二维二分！其实是按中心点行列为界一分为四，代码copy自
  
  https://leetcode.cn/problems/search-a-2d-matrix-ii/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-5-4/
  
  （虽然但是代码和时空复杂度都太高了）
  ```c++
  bool binary_search_matrix(vector<vector<int>>& matrix, int target, int m_1, int m_2, int n_1, int n_2) {
      // check border
      if (m_1 > m_2 || n_1 > n_2) {
        return false;
      }
      if (m_2 - m_1 <= 1 && n_2 - n_1 <= 1) {
        return target == matrix[m_1][n_1] || 
          target == matrix[m_2][n_1] || 
          target == matrix[m_1][n_2] ||
          target == matrix[m_2][n_2];
      }

      int m_mid = (m_1 + m_2) >> 1, n_mid = (n_1 + n_2) >> 1;
      if (target == matrix[m_mid][n_mid]) {
        return true;
      }
      else if (target < matrix[m_mid][n_mid]) {
        return binary_search_matrix(matrix, target, m_1, m_mid - 1, n_mid + 1, n_2) ||//up-right 
          binary_search_matrix(matrix, target, m_mid + 1, m_2, n_1, n_mid - 1) ||//down-left
          binary_search_matrix(matrix, target, m_1, m_mid, n_1, n_mid); //up-left
      }
      else {//target > matrix[m_mid][n_mid]
        return binary_search_matrix(matrix, target, m_1, m_mid - 1, n_mid + 1, n_2) ||//up-right 
          binary_search_matrix(matrix, target, m_mid + 1, m_2, n_1, n_mid - 1) ||//down-left
          binary_search_matrix(matrix, target, m_mid, m_2, n_mid, n_2); //down-right
      }
    }
  public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        return binary_search_matrix(matrix, target, 0, matrix.size() - 1, 0, matrix[0].size() - 1);
    }
  
  ```
  
  
  378. 有序矩阵中第K小的元素
  
  承接自240，从左下或者右上开始走
  
  1.初始位置在matrix[n-1][0]（即左下角）；

  2.设当前位置为matrix[i][j]matrix[i][j]。若matrix[i][j]≤mid，则将当前所在列的不大于mid的数的数量（即i+1）累加到答案中，并向右移动，否则向上移动；

  3.不断移动直到走出格子为止。

  
  不妨假设答案为x，那么可以知道l≤x≤r，这样就确定了二分查找的上下界。

  每次对于「猜测」的答案mid，计算矩阵中有多少数不大于mid：
  
  a.如果数量不少于k，那么说明最终答案x不大于mid；

  b.如果数量少于k，那么说明最终答案x大于mid。

  ```c++
  bool check(vector<vector<int>>& matrix, int mid, int k, int n) {
        int i = n - 1;
        int j = 0;
        int num = 0;
        while (i >= 0 && j < n) {
            if (matrix[i][j] <= mid) {
                num += i + 1;
                j++;
            } else {
                i--;
            }
        }
        return num >= k;
    }

    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        int left = matrix[0][0];
        int right = matrix[n - 1][n - 1];
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (check(matrix, mid, k, n)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

  ```
