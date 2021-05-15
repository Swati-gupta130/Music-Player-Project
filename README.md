# Music-Player-Project

#include<iostream>
#include<string>
#include<stdlib.h>
#include<fstream>

using namespace std;

struct node
{
    char song[100];
    struct node *next;
    struct node *prev;
}*top,*temp,*top1;

void tofile(char a[])
{
    fstream f1;
    f1.open("playlist.txt",ios::out|ios::app);
    f1<<a<<endl;
    f1.close();
}

void add_node(struct node *first)
    {
    char a[100];
    while(first->next!=NULL)
    {
        first=first->next;
    }
    first->next=(struct node*)malloc(sizeof(struct node));
    first->prev=first;
    first=first->next;
    cout<<"\nEnter Song name-  ";
	cin.ignore();
    cin.getline(a,100);
    strcpy(first->song,a);
    tofile(a);
    first->next=NULL;
}

void delete_file(char a[])
{
    fstream f1,f2;
    string line;
    int x=0;
    f1.open("playlist.txt",ios::in|ios::out);
    f2.open("temp.txt",ios::in|ios::out);
    while(!f1.eof())
    {
        getline(f1,line);
        if(strcmp(a,line.c_str())!=0)
        f2<<line<<endl;
        else if (strcmp(a,line.c_str())==0)
        x=1;
    }
   f1.close();
   f2.close();
    remove("playlist.txt");
    rename("temp.txt","playlist.txt");
    if(x==0)
        {
        cout << "There is no such song " << endl;
        }
    else
        {
        cout << "Song has been deleted." << endl;
        }
	
    }

void printlist(struct node *first)
{
    cout<<"\nPlaylist Name- ";
    while(first->next!=NULL)
    {
        cout<<first->song<<endl;
        first=first->next;
    }
    cout<<first->song<<endl;
}

void count_nodes(struct node *first)
{
    int i=0;
    while (first->next!=NULL)
    {
        first=first->next;
        i++;
    }
    i++;
    cout<<"\nTotal songs-  "<<i-1<<endl;
}


void search1(struct node *first)
{
    char song[100];
    cout<<"\nEnter song To be Searched- ";
    scanf("%s",&song);
    int flag=0;

    while(first!=NULL)
    {
        if(strcmp(first->song,song)==0)
        {
            cout<<"\n#Song Found"<<endl;
            flag++;
            break;
        }
        else
        {
            first=first->next;
        }
    }
    if(flag==0)
    {
        cout<<"\n#Song Not found"<<endl;
    }
}

void create()
{
    top = NULL;
}

void push(char data[])
{
    if (top == NULL)
    {
        top =(struct node *)malloc(sizeof(struct node));
        top->next = NULL;
        strcpy(top->song,data);
    }
    else if (strcmp(top->song,data)!=0)
    {
        temp =(struct node *)malloc(sizeof(struct node));
        temp->next = top;
        strcpy(temp->song,data);
        top = temp;
    }
}


void play(struct node *first)
{
    char song[100];
    printlist(first);
    cout<<"\nChoose song you wish to play- ";
    scanf("%s",song);
    int flag=0;

    while(first!=NULL)
    {
        if(strcmp(first->song,song)==0)
        {
            cout<<"\nNow Playing  "<<song<<endl;
            flag++;
            push (song);
			 string a="start "+(string)song+".mp3";
			 system(a.c_str());
            break;
        }
        else
        {
            first=first->next;
        }
    }
    if(flag==0)
    {
        cout<<"\nSong Not found"<<endl;
    }
}


void del_song(struct node *start)
{
    string song;
    printlist(start);
    cout<<"\nChoose song you wish to delete- ";
    cin>>song;
    int flag=0;
    while(start!=NULL)
    {
		if(strcmp(start->song,song.c_str())==0)
        {
            cout<<"\nSong Found"<<endl;
            struct node *temp;
            temp= (struct node *)malloc(sizeof(node));
            temp=start;
            delete_file(temp->song);
            temp->prev->next=temp->next;
            temp->next->prev=temp->prev;
            free(temp);
            flag++;
            break;
        }
        else
        {
            start=start->next;
        }
    }
    if(flag==0)
    {
        cout<<"\nSong Not found"<<endl;
    }
}



int main()
{
    int choice,loc;
    char song[100];
    struct node *top,*hold;
    top=(struct node *) malloc(sizeof(struct node));
    cout<<"\t\t\t\a\a\a\a**WELCOME**"<<endl;
    cout<<"\nEnter your playlist name-  ";
    cin.getline(top->song,100);
    top->next=NULL;
    hold=top;
    create();

    do{
        cout<<"\n1.Add  New Song\n2.Delete Song\n3.Display Playlist\n4.Total Songs\n5.Search Song\n6.Play Song\n7.Exit"<<endl;
        cout<<("\nEnter your choice- ");
        cin>>choice;

        switch(choice)
        {
            case 1:add_node(top);
            break;
            case 2:del_song(top);
            break;
            case 3:printlist(top);
            break;
            case 4:count_nodes(hold);
            break;
            case 5:search1(top);
            break;
            case 6:play(top);
            break;
            case 7:exit(0);
        }

    }while(choice!=0);
return 0;
}
[playlist.txt](https://github.com/Swati-gupta130/Music-Player-Project/files/6487615/playlist.txt)
