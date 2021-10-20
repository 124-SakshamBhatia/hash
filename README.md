#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define max 3
int hashval(int key)
{
    int n;
    n=key%max;
    return n;
}
struct employee{
    int empno;
    char empname[20];
    int empsal;
};
typedef struct employee emp;
struct hashtable
  {
    int key;
    long int addr;
  };
typedef struct hashtable ht;

void insert(FILE *fp,ht *h,int n)
  {
    emp a;
    int hindex,countindex;
    int flag=0;
    int i;
    for(int i=0;i<n;i++)
     {
        printf("enter empno,empname,empsal\n");
        scanf("%d%s%d",&(a.empno),a.empname,&(a.empsal));
        hindex=hashval(a.empno);
        countindex=hindex;
        while(h[hindex].key!=-1)
            {
              hindex=(hindex+1)%max;
              flag=1;
              if(hindex==countindex)
                  {
                      printf("entry is not possible\n");
                      return;
                  }
        }
        h[hindex].key=a.empno;
        fseek(fp,0,SEEK_END);
        h[hindex].addr=ftell(fp);
        fprintf(fp,"%d %s %d\n",(a.empno),a.empname,(a.empsal));
        printf("entry succesful\n");
        if(flag)
        printf("linear probing is used\n");
    }
}
void search(FILE *fp,ht *h,int n)
{
    emp a;
    int hindex,countindex;
    printf("enter the empno of the employee\n");
    scanf("%d",&(a.empno));
    hindex=hashval(a.empno);
    countindex=hindex;
    while(h[hindex].key!=a.empno)
    {
        hindex=(hindex+1)%max;
        if(hindex==countindex)
        {
            printf("unsuccessful search\n");
            return ;
        }
    }
    printf("Successful search\n");
    fseek(fp,h[hindex].addr,SEEK_SET);
    fscanf(fp,"%d%s%d",&(a.empno),a.empname,&(a.empsal));
    printf("%d %s %d\n",(a.empno),a.empname,(a.empsal));
}
void display(FILE *fp,ht *h,int n)
{
    int i=0;
    emp a;
    for(i=0;i<max;i++)
    {
        if(h[i].key!=-1)
        {
            printf("hash table value are |%d %d|\t",h[i].key,h[i].addr);
            fseek(fp,h[i].addr,SEEK_SET);
            fscanf(fp,"%d%s%d",&(a.empno),a.empname,&(a.empsal));
            printf("The values in the secondary storage are %d %s %d\n",a.empno,a.empname,a.empsal);
        }
    }
}
int main()
{
    FILE *fp;
    fp=fopen("hash.txt","w+");
    int n,ch,i;
    ht h[max];
    for(i=0;i<max;i++)
    h[i].key=-1;
    for(;;)
    {
        printf("1.Insert\n2.Display\n3.Search\n4.Exit\n");
        printf("Enter choice\n");
        scanf("%d",&ch);
        switch(ch)
        {
            case 1:
                printf("Enter the number of elements\n");
                scanf("%d",&n);
                insert(fp,h,n);
                break;
            case 3:
                search(fp,h,n);
                break;
            case 2:
                display(fp,h,n);
                break;
            default:
                return 0;
        }
    }
}
