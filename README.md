# PROBLEM STATEMENT
# In Chefland, there is a monthly robots competition. In the competition, a grid table of N rows and M columns will be used to place robots. A cell at row i and column j in the table is called cell (i, j). To join this competition, each player will bring two robots to compete and each robot will be placed at a cell in the grid table. Both robots will move at the same time from one cell to another until they meet at the same cell in the table. Of course they can not move outside the table. Each robot has a movable range. If a robot has movable range K, then in a single move, it can move from cell (x, y) to cell (i, j) provided (|i-x| + |j-y| <= K). However, there are some cells in the table that the robots can not stand at, and because of that, they can not move to these cells. The two robots with the minimum number of moves to be at the same cell will win the competition.

Chef plans to join the competition and has two robots with the movable range K1 and K2, respectively. Chef does not know which cells in the table will be used to placed his 2 robots, but he knows that there are 2 cells (1, 1) and (1, M) that robots always can stand at. Therefore, he assumes that the first robot is at cell (1, 1) and the other is at cell (1, M). Chef wants you to help him to find the minimum number of moves that his two robots needed to be at the same cell and promises to give you a gift if he wins the competition.
Input

The first line of the input contains an integer T denoting the number of test cases. The description of T test cases follows.

    The first line of each test case contains 4 space-separated integers N M K1 K2 denoting the number of rows and columns in the table and the movable ranges of the first and second robot of Chef.
    The next N lines, each line contains M space-separated numbers either 0 or 1 denoting whether the robots can move to this cell or not (0 means robots can move to this cell, 1 otherwise). It makes sure that values in cell (1, 1) and cell (1, M) are 0.

Output

For each test case, output a single line containing the minimum number of moves that Chef’s 2 robots needed to be at the same cell. If they can not be at the same cell, print -1.
Constraints

    1 ≤ T ≤ 10
    1 ≤ N, M ≤ 100
    0 ≤ K1, K2 ≤ 10

Subtasks

Subtask #1 : (25 points)

    K1 = K2 = 1

Subtask # 2 : (75 points)

    Original Constraints 

Example

Input:
2
4 4 1 1
0 1 1 0
0 1 1 0
0 1 1 0
0 0 0 0
4 4 1 1
0 1 1 0
0 1 1 0
0 1 1 0
1 0 0 1

Output:
5
-1

Explanation

Example case 1. Robot 1 can move (1, 1) -> (2, 1) -> (3, 1) -> (4, 1) -> (4, 2) -> (4, 3), and robot 2 can move (1, 4) -> (2, 4) -> (3, 4) -> (4, 4) -> (4, 3) -> (4, 3), they meet at cell (4, 3) after 5 moves.

Example case 2. Because the movable range of both robots is 1, robot 1 can not move from (3, 1) to (4, 2), and robot 2 can not move from (3, 4) to (4, 3. Hence, they can not meet each other.

******************************************************************************************************************************

# SOLUTION

    include <iostream>
    include <cmath>
    using namespace std;
 
    void recursea(int **g,int a,int b,int n,int m,int k);
 
    int main()
    {
        ios_base::sync_with_stdio(false);
        int t,n,m,k1,k2,i,j,min1,z,**g1,**g2,**g3,**g4,l,o;
        cin>>t;
        while(t--)
        {
            cin>>n>>m>>k1>>k2;
            min1=10000;
            z=0;
            g1=new int*[n];
            g2=new int*[n];
            g3=new int*[n];
            g4=new int*[n];
            for(i=0;i<n;i++)
            {
                g1[i]=new int[m];
                g2[i]=new int[m];
                g3[i]=new int[m];
                g4[i]=new int[m];
                for(j=0;j<m;j++)
                {
                    cin>>g1[i][j];
                    g2[i][j]=g1[i][j];
                    g3[i][j]=0;
                    g4[i][j]=0;
                }
            }
            if(m==1)
                cout<<"0\n";
            else
            {
                g1[0][0]=-1;
                g2[0][m-1]=-1;
                g3[0][0]=1;
                g4[0][m-1]=1;
                if(k1!=0)
                {
                    recursea(g1,0,0,n,m,k1);
                    o=1;
                    for(i=2;(i<=n*m)&&(g3[0][m-1]==0)&&(o==1);i++)
                    {
                        o=0;
                        for(j=0;(j<=k1*(i-1))&&(j<n);j++)
                        {
                            for(l=0;(l<=k1*(i-1))&&(l<m);l++)
                            {
                                if(g1[j][l]==i)
                                {
                                    o=1;
                                    g3[j][l]=1;
                                    recursea(g1,j,l,n,m,k1);
                                }
                            }
                        }
                    }
                }
                if(k2!=0)
                {
                    recursea(g2,0,m-1,n,m,k2);
                    o=1;
                    for(i=2;(i<=n*m)&&(g4[0][0]==0)&&(o==1);i++)
                    {
                        o=0;
                        for(j=0;(j<=k2*(i-1))&&(j<n);j++)
                        {
                            for(l=m-1;(l>=(m-1)-(k2*(i-1)))&&(l>=0);l--)
                            {
                                if(g2[j][l]==i)
                                {
                                    o=1;
                                    g4[j][l]=1;
                                    recursea(g2,j,l,n,m,k2);
                                }
                            }
                        }
                    }
                }
                for(i=0;i<n;i++)
                {
                    for(j=0;j<m;j++)
                    {
                        if(((i==0)&&(j==0))||((i==0)&&(j==m-1)))
                        {
                            if((g1[i][j]==-1)&&(g2[i][j]==0));
                            else if((g1[i][j]==0)&&(g2[i][j]==-1));
                            else
                            {
                                if(max(g1[i][j],g2[i][j])-1<min1)
                                    min1=max(g1[i][j],g2[i][j])-1;
                                z=1;
                            }
                        }
                        else
                        {
                            if(g1[i][j]==1);
                            else
                            {
                                if((g1[i][j]!=0)&&(g2[i][j]!=0))
                                {
                                    if(max(g1[i][j],g2[i][j])-1<min1)
                                        min1=max(g1[i][j],g2[i][j])-1;
                                    z=1;
                                }
                            }
                        }
                    }
                }
                if(z==1)
                    cout<<min1<<"\n";
                else
                    cout<<"-1\n";
            }
        }
        return 0;
    }
    void recursea(int **g,int a,int b,int n,int m,int k)
    {
        int i,j,x,y;
        x=a-k;
        y=b-k;
        if(x<0)
            x=0;
        if(y<0)
            y=0;
        for(i=x;(i<=a+k)&&(i<n);i++)
        {
            for(j=y;(j<=b+k)&&(j<m);j++)
            {
                if((i==a)&&(j==b));
                else
                {
                    if(abs(i-a)+abs(j-b)<=k)
                    {
                        if(g[a][b]==-1)
                        {
                            if(g[i][j]==0)
                                g[i][j]=2;
                            else if(g[i][j]==1);
                            else if(g[i][j]>2)
                                g[i][j]=2;
                        }
                        else
                        {
                            if(g[i][j]==1);
                            else if(g[i][j]==0)
                                g[i][j]=g[a][b]+1;
                            else if(g[i][j]>g[a][b]+1)
                                g[i][j]=g[a][b]+1;
                        }
                    }
                }
            }
        }
    }
