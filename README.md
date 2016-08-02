# PROBLEM STATEMENT
# https://www.codechef.com/JULY16/problems/CHEFARC

# SOLUTION
#include <iostream>
#include <cmath>
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
