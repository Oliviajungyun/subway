#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <wchar.h>

struct node
{
 char num[20];
 char name[30];
 char ho[20];
 struct node *next;
};
struct node *head=0;
struct node graph[607];

void addEdge(char *v1, char *v2, char *time) // doReverse == 1, yes, otherwise 0.
{
 
	char v;

    struct node *temp = (struct node *) malloc(sizeof(struct node)); 
	 strcpy(temp->num,v2);
    temp->next = 0;
	//printf("%s",v1);
	v=findstation(v1);//v1의 인덱스 번호를 찾습니다.

 if (graph[v].next == 0)    // 맨 처음붙는 경우
 {
  graph[v].next = temp;
 } 
  else          // 이미 붙어있는 것이 있는 경우,
  {
  struct node *cur = graph[v].next;

   while (cur->next != 0)    // 맨 끝을 찾아서
   { 
    cur=cur->next;
   }
    cur->next=temp;
  }
}

int findstation(char *v)//몇번째배열에 인덱스값인지 찾아서 반환해줍니다.
{
  char x = 0;

 while(1)
 {
  if(strcmp(graph[x].name,v)==0)
  {
   return x;
  }
 x++;
 }
}
 
void showConnected(int v)
{
    struct node *temp = graph[v].next;
    while (temp != 0)
	{
  printf("%d --> n", temp->num);
 temp = temp->next;
	}
 
}

void addToSLL(char *num,char *name,char *ho)
{
 struct node *d_new=(struct node*)malloc(sizeof(struct node));

 strcpy(d_new->num,num);
 strcpy(d_new->name,name);
 strcpy(d_new->ho,ho);
 d_new->next=0;
 if(head==0)
 {
  head=d_new;
  return;
 }
 else
 {
  struct node *last=0;
  last=head;
  while(last->next !=0)
  {
   last=last->next;
  }

  last->next=d_new;
  return;
 }

}
void printSLL()
{
 struct node *cur=head;
 while(cur!=0)
 {
  printf("%s,%s, %s \n",cur->num,cur->name,cur->ho);
  cur=cur->next;
 }
}
int main()
{ 
 FILE *f=0;
 int cnt=0;
 char subway1[20],subway2[20];
 char read[100]={0,};
 char time[5];
 char *Rd;
 char test=0;                      
 f =fopen("subway.txt","r+");
 while(fgets(read,sizeof(read),f))//파일을 읽어와 원하는부분을 잘라 각각의변수에 저장합니다.
 {
  if(strcmp(read,"\n")==0)
  {
   break;
  }
  Rd=strtok(read," ");

  strcpy(graph[cnt].num,Rd);//지하철의 번호
  Rd=strtok(NULL," ");
  strcpy(graph[cnt].name,Rd);//지하철의 이름
  Rd=strtok(NULL,"\n");
  strcpy(graph[cnt].ho,Rd);//지하철의호선

  addToSLL(graph[cnt].num,graph[cnt].name,graph[cnt].ho);//각각 저장한변수들을 SLL에저장합니다.
  cnt++;
 }

 while(fgets(read,sizeof(read),f))//각 역 사이의 거리를 받아오는 부분입니다.
 {
  
  Rd=strtok(read," ");
  strcpy(subway1,Rd);

  Rd=strtok(NULL," ");
  strcpy(subway2,Rd);

  Rd=strtok(NULL,"\n");
  strcpy(time,Rd);

  addEdge(subway1,subway2,time); //subway1을기준으로 옆으로 붙여나갑니다.
 }

 //printSLL();

 //showConnected(100);
 fclose(f);
}
