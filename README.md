# C-MiniProject
#include<stdio.h>
#include<stdlib.h>
#include<stdio.h>
#include<string.h>
#include<ctype.h>
#include<windows.h>
#include<dirent.h>
#include<unistd.h>
#include<sys/stat.h>
int countlines;
int isFolderExists(const char*p);
void view_folder(char un[],char pwd[]);
void login();
void reg();
void menu(char un[],char pwd[]);
void copy_file(char un[],char pwd[]);
void display_file(char un[],char pwd[]);
void append_file(char un[],char pwd[]);
void create_file(char un[],char pwd[]);
void edit_file(char un[],char pwd[]);
void delete_file_per(char un[],char pwd[]);
void retrieve_file(char un[],char pwd[]);
void rename_file(char un[],char pwd[]);
void view_history(char un[],char pwd[]);
void write_data(char un[],char pwd[]);
void unregister(char un[],char pwd[]);
void his(char un[],char pwd[]);
struct details
{
	char username[16];
	char password[16];
}*user;
struct date
{
	char d[50];
}*dateu;

void SetColor(int ForgC)
{
	WORD wColor;
	HANDLE hStdOut=GetStdHandle(STD_OUTPUT_HANDLE);
	CONSOLE_SCREEN_BUFFER_INFO csbi;
	if(GetConsoleScreenBufferInfo(hStdOut,&csbi)){
		wColor=(csbi.wAttributes & 0xF0)+(ForgC & 0x0F);
		SetConsoleTextAttribute(hStdOut,wColor);
	}
}
void cls(HANDLE hConsole)
{
    CONSOLE_SCREEN_BUFFER_INFO csbi;
    SMALL_RECT scrollRect;
    COORD scrollTarget;
    CHAR_INFO fill;

    // Get the number of character cells in the current buffer.
    if (!GetConsoleScreenBufferInfo(hConsole, &csbi))
    {
        return;
    }

    // Scroll the rectangle of the entire buffer.
    scrollRect.Left = 0;
    scrollRect.Top = 0;
    scrollRect.Right = csbi.dwSize.X;
    scrollRect.Bottom = csbi.dwSize.Y;

    // Scroll it upwards off the top of the buffer with a magnitude of the entire height.
    scrollTarget.X = 0;
    scrollTarget.Y = (SHORT)(0 - csbi.dwSize.Y);

    // Fill with empty spaces with the buffer's default text attribute.
    fill.Char.UnicodeChar = TEXT(' ');
    fill.Attributes = csbi.wAttributes;

    // Do the scroll
    ScrollConsoleScreenBuffer(hConsole, &scrollRect, NULL, scrollTarget, &fill);

    // Move the cursor to the top left corner too.
    csbi.dwCursorPosition.X = 0;
    csbi.dwCursorPosition.Y = 0;

   SetConsoleCursorPosition(hConsole, csbi.dwCursorPosition);
}
void clearing()
{
	HANDLE hStdout;
	hStdout=GetStdHandle(STD_OUTPUT_HANDLE);
	cls(hStdout);
}
void instructions_username()
{
	SetColor(2);
	printf("\n\t\t\t\t\tRULES FOR CHOOSING A APPROPRIATE USERNAME\n");
	SetColor(13);
	printf("\t\t\t\t1.The username should contain atleast 8 characters.\n\t\t\t\t2.The username should not exceed 16 characters.\n\t\t\t\t3.The username should not contain space.\n\t\t\t\t4.The username can contain alphanumeric characters and even special characters.\n");
}
void instructions_password()
{
	SetColor(2);
	printf("\n\t\t\t\t\tRULES FOR CHOOSING APPROPRIATE PASSWORD\n");
	SetColor(13);
	printf("\t\t\t\t1.The password should contain atleast 8 characters.\n\t\t\t\t2.The username must contain atleast a uppercase letter.\n\t\t\t\t3.The password must contain atleast a lowercase letter.\n\t\t\t\t4.The password must contain atleast a digit.\n\t\t\t\t5.The password must contain atleat one special character like !,@,#,$,^,&,*,%\n");
}
void display()
{
	if(getch()==13)
		clearing();
	SetColor(2);
	printf("\n************************************************************************************************************************");
	SetColor(14);
	printf("\n\t\t\t\t\tWELCOME TO SECURE TEXT EDITOR\n");
	SetColor(2);
	printf("\n************************************************************************************************************************\n");
	SetColor(15);
}
int main()
{
	display();
	int choice;
	SetColor(9);
	printf("\t\t\t\t\tPRESS 1 FOR LOGIN \n\t\t\t\t\tPRESS 2 FOR REGISTER\n");
	SetColor(12);
CH:	printf("\n\t\t\t\t\tENTER:");
	scanf("\t\t\t\t\t%d",&choice);
	SetColor(15);
	switch(choice)
	{
		case 1:login();break;
		case 2:reg();break;
		default:{printf("\n\t\t\t\t\tINVLAID CHOICE\n");
				goto CH;
			};break;
	}
	printf("\n\nPRESS ENTER\n");
	if(getch()==13)
		clearing();
	
	
	return 0;
}
int check_password(char s[])
{
	int i,d=0,l=0,u=0,sp=0;
	for(i=0;i<strlen(s);i++)
	{
		if(isdigit(s[i]))
			d++;
		else if(isupper(s[i]))
			u++;
		else if(islower(s[i]))
			l++;
		else if(s[i]=='!'||s[i]=='@'||s[i]=='#'||s[i]=='$'||s[i]=='*'||s[i]=='^'||s[i]=='&'||s[i]=='%')
			sp++;
	}
	if(d>=1&&u>=1&&l>=1&&sp>=1&&strlen(s)>7)
		return 1;
	else
		return 0;
}
int check_username(char s[])
{
	if(strlen(s)<7||strlen(s)>16)
		return 0;
	else
		return 1;
}
void folder_creation(char un[])
{
	
	char s[40]="Accounts_Storage_Folder";
	mkdir(s);
	char path[100],de[100];
	getcwd(path,sizeof(path));
        getcwd(de,sizeof(de));
       
	strcat(path,"\\Accounts_Storage_Folder");
       
	char d[40]="Deleted_Files_Folder";
        strcat(de,"\\Deleted_Files_Folder");
       
	int check;
	mkdir(d);
	chdir(s);
	check=mkdir(un);
     //if(!SetFileAttributes(path,FILE_ATTRIBUTE_HIDDEN));
     //if(!SetFileAttributes(de,FILE_ATTRIBUTE_HIDDEN));
   
	if(!check)
	{
		printf("\n\t\t\t\t\tYOU HAVE CREATED YOUR FOLDER SUCCESSFULLY\n");
	}
	else
	{
		printf("\t\t\t\t\tERROR WHILE RENAMING THE FILE\n");
		folder_creation(un);
	}
	chdir("..");
	chdir(d);
	mkdir(un);
	chdir("..");
	
}

void login()
{
	int key;
	printf("\t\t\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	
	SetColor(14);
	printf("/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*\n");
	SetColor(9);
	printf("\t\t\t\t\tWELCOME TO LOGIN ZONE\n");
	printf("\n\t\t\t\tNOTE:IF DETAILS ARE INCORRECT YOU WILL BE REDIRECTED  TO LOGIN\n");
	FILE *fp1;int count=0,k=0;
	
	fp1=fopen("log_re.dat","r");
	fclose(fp1);
	FILE *fp;
	fp=fopen("log_re.dat","r");

	if(fp==NULL)
		printf("file not found");
	char u[16],pwd[16];
	int c,d;
	
	user=(struct details *)malloc(sizeof(struct details));
	SetColor(11);
	printf("\n\t\t\t\tENTER USERNAME:");
	SetColor(13);
	scanf("%s",u);
	SetColor(11);
	printf("\n\t\t\t\tENTER PASSWORD:");
	SetColor(13);
	int pcheck=0;
	do
	{
		 pwd[pcheck]=getch();
                
              
		if(pwd[pcheck]!='\r')
			printf("*");
		pcheck++;
	}while(pwd[pcheck-1]!='\r');
	pwd[pcheck-1]='\0';
	SetColor(15);
	int iter;
	for(iter=0;iter<strlen(pwd);iter++)
	{
		pwd[iter]=pwd[iter]+100;
	}
	while(!feof(fp))
	{
		fread(user,sizeof(struct details),1,fp);
		
		if(strcmp(user->username,u)==0)
		{
			
			k++;

			if(strcmp(user->password,pwd)==0)
			{
				
				count++;
				
			
			}
		}
		
		
	}
	if(count>0)
	{
		SetColor(14);
		printf("\n\n\t\t\t\tLOGIN SUCCESS\n");
		SetColor(15);
		for(iter=0;iter<strlen(pwd);iter++)
			pwd[iter]=pwd[iter]-100;
		write_date(u,pwd);
		
			
		
	}
	else if(k>0 && count<1)
	{
		SetColor(10);
		printf("\n\n\t\t\tPASSWORD IS INCORRECT REDIRECTING TO LOGIN\n");
		login();
	}
	else if(count<1)
	{
			
			SetColor(13);
			printf("\n\n\t\t\tUSERNAME NOT FOUND\n");
			SetColor(10);
			printf("\n\n\t\t\tWOULD YOU LIKE TO RELOGIN?");
			SetColor(9);
			printf("\n\n\t\t\tENTER 1 TO LOGIN:");
			scanf("%d",&d);
			if(d==1)
			{
				login();
			}
			else
			{
				SetColor(10);
			printf("\n\n\t\t\tPRESS 1 TO REGISTER:");
			scanf("%d",&c);
			if(c==1)
				reg();

			}	
				
		
		}

fclose(fp);
}
int check_user(char un[])
{
	FILE *fp;int count=0;
	fp=fopen("log_re.dat","r");
	while(!feof(fp))
	{
		fread(user,sizeof(struct details),1,fp);
		if(strcmp(user->username,un)==0)
			count++;
	}
	fclose(fp);
	if(count>0)
	{
		return 0;
	}
	else
		return 1;
}

void reg()
{
	int iter;
	printf("\n\t\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(14);
	printf("\n/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*\n");
	SetColor(10);
	printf("\t\t\t\t\tWELCOME TO REGISTRATION\n");
	FILE *fp,*io;int c,checku,checkp;char un[16],p[16];
	fp=fopen("log_re.dat","a+");
	char path[100];
	getcwd(path,sizeof(path));
	strcat(path,"\\log_re.dat");
	//if(!SetFileAttributes(path,FILE_ATTRIBUTE_HIDDEN))
		//printf("not hidden");
	
	user=(struct details *)malloc(sizeof(struct details));
	
	SetColor(13);
	int count=0;instructions_username();
	SetColor(11);
UN:	printf("\n\t\t\tENTER USERNAME:");
	SetColor(10);
	scanf("%s",un);
	checku=check_username(un);
	if(checku==0)
	{
		SetColor(12);
		printf("\n\t\t\tENTER A VALID USERNAME\n");
		SetColor(11);
		goto UN;
	}
	if(check_user(un)==0)
	{
		SetColor(12);
		printf("\n\t\t\tUSERNAME ALREADY EXISTS\n");
		goto UN;
	}
	else
		strcpy(user->username,un);
	SetColor(10);
	
	instructions_password();


PD:	SetColor(11);
	printf("\n\t\t\tENTER PASSWORD:");
	SetColor(13);
	scanf("%s",p);

	checkp=check_password(p);
	if(checkp==0)
	{
		SetColor(12);
		printf("\n\t\t\tENTER A VALID PASSWORD\n");
		goto PD;
	}
	else{
		for(iter=0;iter<strlen(p);iter++)
			p[iter]=p[iter]+100;
		strcpy(user->password,p);
	}

	fwrite(user,sizeof(struct details),1,fp);
	fclose(fp);
	SetColor(11);
	printf("\n\t\t\t\tPRESS ENTER\n");
	while(getch()==13)
	{
		printf("\t\t\tSUCCESSFULLY REGISTERED\n");
		folder_creation(un);
		SetColor(10);
		printf("\n\t\t\t\t\tTHANKYOU FOR YOUR REGISTRATION HAVE A NICE DAY :)\n");
		printf("\n\t\t\t\t\tPRESS ANY KEY TO LOGIN\n");
		SetColor(14);
		printf("\n/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/*/\n");
	}
	SetColor(11);
	login();
	
	
	
	
}
int isFolderExists(const char *p)
{
	chdir("..");
	char path[100];
	
	chdir("Accounts_Storage_Folder");
	struct stat stats;
	stat(p,&stats);
	if(S_ISDIR(stats.st_mode))
		return 1;
	else
		return 0;
}
void view_folder(char us[],char p[])
{
	char path[100];
	int key;
	
	char un[30],pwd[16],check[100];
	SetColor(9);
	printf("\n\nPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(14);
	printf("\n\t\t\t\t\tFOLDER\n");
	char a[26]="Accounts_Storage_Folder";
	DIR *d;
	struct dirent *dir;
VUN:	SetColor(13);	
	printf("\n\t\t\tEnter Username:");
	SetColor(10);
	scanf("%s",un);
	//printf("%s",p);
	if(strcmp(un,us)==0)
	{
	
		int count=0,k=0,j;
		chdir(a);
	
	if(isFolderExists(us))
	{
		
		SetColor(9);
		printf("\n\tFOLDER EXISTS AND CONTENTS OF THE FOLDER ARE\n");
		chdir(us);
		d=opendir(".");
		if(d)
		{
			while((dir=readdir(d))!=NULL)
			{
				count++;
				if(count>2)
				{
					k++;
					SetColor(13);
					printf(" %d\t%s\n\n",k,dir->d_name);
					
				}
			}
			
			printf("PRESS 1 TO REDIRECT TO MENU:");
			scanf("%d", &j);
			if(j==1)
			{
				//printf("%s",p);
				menu(us,p);
			}
			
                         chdir("..");
		}
	}

	}
	else
	{
		SetColor(10);
		printf("\n\n\t\t\tUSERNAME NOT MATCHED\n");
		SetColor(9);
		printf("\n\n\tPRESS 1 TO REENTER THE USERNAME OR ELSE 2 TO  REDIRECT TO MENU");
		scanf("%d",&key);
		if(key==1)
			goto VUN;
		else if(key==2)
		{
		//	printf("%s",p);
			menu(un,p);
		}
		else
		{
		//	printf("%s",p);
			menu(un,p);
		}
	}
	
	

	
}
void menu(char un[],char pwd[])
{
	SetColor(9);
	printf("\n\t\t\tPRESS ENTER\n");

	if(getch()==13)
		clearing();
	SetColor(14);
	printf("\n\t\t\t\t\tWELCOME TO THE MENU\n");
	SetColor(10);
       	printf("\n\tPRESS 1 FOR NEW FILE\n\n\tPRESS 2 FOR COPY\n\n\tPRESS 3 FOR DISPLAY A FILE\n\n\tPRESS 4 FOR EDITING A FILE\n\n\tPRESS 5 FOR VIEW FOLDER\n\n\tPRESS 6 FOR RENAMING A FILE\n\n\tPRESS 7 FOR APPENDING A FILE\n\n\tPRESS 8 FOR DELETING A FILE PERMANANTLY\n\n\tPRESS 9 FOR RETRIEVING A DELETED FILE\n\n\tPRESS 10 FOR VIEW HISTROY\n\n\tPRESS 11 FOR UNREGISTER\n\n\tPRESS 12 FOR LOGOUT\n");
	SetColor(1);
	int choice=0;

ME:	printf("\n\t\t\tENTER YOUR CHOICE :");

	scanf(" %d", &choice);

	SetColor(15);

	switch(choice)
	{
		case 1:create_file(un,pwd);break;
		case 2:copy_file(un,pwd);break;
		case 3:display_file(un,pwd);break;
		case 4:edit_file(un,pwd);break;
		case 5:{
			       view_folder(un,pwd);
			       
		       }break;
		case 6:rename_file(un,pwd);break;
		case 7:append_file(un,pwd);break;
		case 12:{
			       SetColor(14);
			       clearing();
			       printf("\n\t\t\tPLEASE WAIT FOR FEW MOMENTS.....\n");
			       clearing();
				SetColor(12);
			       printf("\n\t\t\tLOGGING OUT FROM THE APPLICATION\n\n");
			       SetColor(15);
                               exit(0);
			       SetColor(15);
		       }
		
		case 8:delete_file_per(un,pwd);break;
		case 9:retrieve_file(un,pwd);break;
		case 10:view_history(un,pwd);break;
		case 11:unregister(un,pwd);break;
		default:{
				printf("\nENTER A VALID CHOICE\n");
				goto ME;
			}break;

	}

	
	
}
void create_file(char un[],char pwd[])
{
	char s[100];
	
	SetColor(1);
	printf("\n\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(14);
	printf("\n\t\t\tNEW FILE\n");
	
	char file_name[25];
	SetColor(11);
	printf("\nENTER NAME FOR THE FILE:");
	SetColor(15);
	scanf("%s",file_name);
	FILE *fp,*dp;
	char *str[100];
	
	char path[100];
	chdir("..");
	chdir("..");
	getcwd(path,sizeof(path));
	
	char path2[100];
	strcpy(path2,path);
	strcat(path,"\\Accounts_Storage_Folder\\");
	strcat(path2,"\\Deleted_Files_Folder\\");
	strcat(path,un);
	strcat(path2,un);
	
	strcat(path,"\\");
	strcat(path2,"\\");

	strcat(path,file_name);
	strcat(path2,file_name);

	
	fp = fopen(path,"w");
	dp=fopen(path2,"w");
	if(fp==NULL)
		printf("Error while opening the file\n");
	
	SetColor(10);
	printf("AFTER YOU ARE DONE WITH WRITING PRESS CTRL + Z AND THEN PREESS ENTER\n");
	printf("\nPRESS ENTER TO START WRITING\n");
	if(getch()==13)
		clearing();
	SetColor(15);
	char c;
	/*while(fgets(str,sizeof(str),stdin))
	{
		fputs(str,fp);
		fputs(str,dp);
	}*/
	while((c=getchar())!=EOF)
	{
		c=c+100;
		fputc(c,fp);
		fputc(c,dp);
	}
	fclose(fp);
	fclose(dp);
	SetColor(13);
	printf("\n\t\t\t\t\tSUCCESSFULLY WRITTEN\n");
	SetColor(15);
      
	menu(un,pwd);
}
void display_file(char un[],char pwd[])
{       

	//int countdf=0;
	char path[100];
	//printf("%s",getcwd(path,sizeof(path)));
	int iter;
	countlines=0;
	int key;
	int pd;
	char p[100];
	SetColor(14);
	printf("\n\t\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	printf("\t\t\t\t\tDISPLAY\n");
	//printf("%s",pwd);
	chdir("Accounts_Storage_Folder");
	chdir(un);
DPD:	SetColor(13);
	printf("\n\t\t\tENTER PASSWORD:");
	SetColor(10);
	scanf("%s",p);
	/*
	for(iter=0;iter<strlen(p);iter++)
	{
		p[iter]=p[iter]+100;
	}*/
	if(strcmp(p,pwd)!=0)
	{
		printf("%s",getcwd(path,sizeof(path)));
		SetColor(4);
		printf("Invalid Password");
		SetColor(9);
		printf("\n\tPRESS 1 TO RENTER THE PASSWORD OR ELSE YOU WILL BE REDIRECTED TO MENU : ");
		SetColor(13);
		scanf("%d",&pd);
		if(pd==1)
			goto DPD;
		else
			menu(un,pwd);
	}
	
	
		//countdp++;
	char file_name[25];
	SetColor(11);
FN:	printf("\nENTER THE FILE NAME TO DISPLAY:");
	scanf("%s",file_name);
	FILE *fp;
	char str[80]={0};
	int Max=80,count=1;
	fp=fopen(file_name,"r");
	
	if(fp==NULL)
	{SetColor(12);
		printf("NO FILE EXISTS\n");
                SetColor(10);
                printf("PRESS 1 TO RETYPE THE FILE NAME\nPRESS 2 TO GOTO MENU\n");
                scanf("%d",&key);
                if(key==1)
                      
		goto FN;
                else 
               {  
                 SetColor(12);
                printf(">>>REDIRECTORING TO MENU");
                menu(un,pwd);}
	}
	else
	{
		
		SetColor(13);
		printf("\nTHE CONTENTS OF THE FILE ARE\n");
		//fgets(str,Max,fp);
		
		SetColor(15);

		char c;
		c=fgetc(fp);
		while(c!=EOF)
		{
			c=c-100;
			printf("%c",c);
			if(c=='\n')
			{
				printf("%d|",count);
				count++;
				countlines++;
			}
			c=fgetc(fp);
			

		}
			

	}
	fclose(fp);
	SetColor(13);
	int cd;
	printf("\n\nWOULD YOU LIKE TO GO TO MENU(Press 1):");
	scanf("%d",&cd);
	if(cd==1)
		menu(un,pwd);

	

}
void edit_file(char un[],char pwd[])
{       int key,c,iter;
	SetColor(9);
	printf("\t\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(14);
	printf("\n\t\t\t\t\tEDITING THE FILE\n");
	chdir("Accounts_Storage_Folder");
	chdir(un);
	char file_name[25],str[100];
	char newln[100],temp[]="temp.txt";
	SetColor(13);
   ZS:printf("\nENTER THE FILE NAME TO EDIT THE FILE:");
	SetColor(10);
	scanf("%s",file_name);
	SetColor(15);
	FILE *fp1,*fp2;
	int line_number,linectr=0,max=100;
	fp1=fopen(file_name,"r");
	fp2=fopen(temp,"r+");
	if(fp1==NULL)
	{       SetColor(12);
		printf("\nFILE  DOESNT EXISTS\n");
                SetColor(10);
                 printf("PRESS 1 TO RETYPE THE FILE NAME\nPRESS 2 TO GOTO MENU\n");
                scanf("%d",&key);
                if(key==1)
                      
		goto ZS;
                else 
                 {SetColor(12);
		printf(">>>REDIRECTORING TO MENU");
                menu(un,pwd);}
	}
	else
	{
		display_file(un,pwd);
		//int line_number,lilnectr=0,max=100;
		SetColor(9);
	  	printf("\nEnter the line number to edit:");
		SetColor(13);
		scanf("%d",&line_number);
		if(line_number<1 || line_number>countlines)
		{
			SetColor(12);
			printf("\n\nENTERED INVALID LINE NUMBER\n");
			SetColor(9);
			//goto AB;
			menu(un,pwd);
		}
		else
		{		
		SetColor(9);
		printf("\nEnter the text:");
		SetColor(13);
		scanf("%s",newln);
	//	printf("\nThe encrypted text is\n%s\n",newln);
		//line_number++;
		//printf("%d",line_number);
		while(!feof(fp1))
		{

			fgets(str,max,fp1);
			if(!feof(fp1))
			{
				linectr++;
				if(linectr!=line_number)
				{
					fprintf(fp2,"%s",str);
				}
				if(line_number == linectr)
				{
					for(iter=0;iter<strlen(newln);iter++){
						newln[iter]=newln[iter]+100;}
					
					fprintf(fp2,"%s\n",newln);
				}
			}
		}
		}
	}
	
        fclose(fp1);
	fclose(fp2);
	SetColor(15);
	printf("\nEDITTED SUCCESSFULLY\n");
        SetColor(12);
	
	remove(file_name);
    
	rename(temp,file_name);
	SetColor(13);
	printf("\nREDIRECTING TO MENU\n");
	menu(un,pwd);
}
void copy_file(char un[],char pwd[])
{  int key;
	SetColor(14);
	printf("\n\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();

	chdir("Account_Storage_Folder");
	chdir(un);
	FILE *fp1,*fp2;
	char file_name[25],copy_file_name[25];
	SetColor(13);
CF:	printf("\nENTER THE NAME OF THE FILE (CONTENT TO BE COPIED FROM)");
	SetColor(9);
	scanf("%s",file_name);

	fp1=fopen(file_name,"r");
	
	if(fp1==NULL)
	{
		SetColor(3);
		printf("FILE DOESNT EXIST\n");
              SetColor(10);
                 printf("PRESS 1 TO RETYPE THE FILE NAME\nPRESS 2 TO GOTO MENU\n");
                scanf("%d",&key);
                if(key==1)
                      
		goto CF;
                else 
                 {SetColor(12);
		printf(">>>REDIRECTORING TO MENU");
                menu(un,pwd);}
		
	}
	else
	{
		SetColor(13);
		printf("ENTER THE NAME OF THE NEW FILE:");
		SetColor(9);
		scanf("%s",copy_file_name);
		fp2=fopen(copy_file_name,"w");
		
		char c;
		int pos;
		fseek(fp1,0L,SEEK_END);
		pos=ftell(fp1);
		fseek(fp1,0L,SEEK_SET);
		while(pos--)
		{
			c=fgetc(fp1);
			fputc(c,fp2);
		}
	}
	fclose(fp1);
	fclose(fp2);
	SetColor(14);
	printf("\n\nCONTENTS COPIED SUCCESSFULLY\n");
	printf("\n\nREDIRECTING TO MENU\n");
	menu(un,pwd);

}
void append_file(char un[],char pwd[])
{  int key,iter;
	char c;
	SetColor(14);
	printf("\n\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(13);
	printf("\n\t\t\t\tAPPENDING A FILE\n");
	chdir("Accounts_Storage_Folder");
	chdir(un);
	FILE *fp;
	char file_name[25],*str[250];
	SetColor(9);
AF:	printf("Enter the Name of the File you want to append:");
	scanf("%s",file_name);
	display_file(un,pwd);
	fp= fopen(file_name,"a");
	if(fp==NULL)
	{
		printf("FILE DOESNT EXIST\n");
              SetColor(10);
                 printf("PRESS 1 TO RETYPE THE FILE NAME\nPRESS 2 TO GOTO MENU\n");
                scanf("%d",&key);
                if(key==1)
                      goto AF;
                else 
                 {SetColor(12);
		printf(">>>REDIRECTORING TO MENU");
                menu(un,pwd);}
	}
	else
	{
		SetColor(9);
		printf("\nEnter the Text you want to Append\n");
		SetColor(15);
		/*while(fgets(str,sizeof(str),stdin))
		{
			for(iter=0;iter<strlen(str);iter++)
			{
		
				fputc(str[iter]+100,fp);
			}

			

		}*/
		while((c=getchar())!=EOF)
		{
			c=c+100;
			fputc(c,fp);
		}
		SetColor(13);
	printf("\nAppended Successfully\n");
	}
	fclose(fp);
	SetColor(9);
	printf("\n\t\t\tREDIRECTING TO MENU\n");
	menu(un,pwd);
}
void rename_file(char un[],char pwd[])
{       char path[100],old[100],new[100];
	printf("%s",getcwd(path,sizeof(path)));
	chdir(un);

	int lp;
	SetColor(9);
	char fn[25],str[25];
	printf("\n\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(14);
RNF:	printf("\nEnter the Name of the file to rename:");
	SetColor(13);
	scanf("%s",fn);
        strcat(old,fn);
	
       printf("\nEnter the new name for the file:");
		SetColor(13);
		scanf("%s",str);
              strcat(new,str);
         
         if ( rename(fn,str)==0 )
    {  SetColor(12);
        printf("FILE IS RENAMED SUCCESSFULLY\n");
    }
    else
    { SetColor(4);
        printf("UNABLE TO RENAME PLEASE CHECK THE EXISTANCE AND CORRECT NAMES OF FILES\n");
         SetColor(9);
        printf("PRESS:1-TO ENTER VAILD FILE NAME\n 2-MENU");
                     int k;
                      scanf("%d",&k);
                     if(k==1)
                        goto RNF;
    }
	SetColor(13);
	printf("\n\t\t\t REDIRECTING TO MENU\n");
	chdir("..");
	menu(un,pwd);
	
}
void delete_file_per(char un[],char pwd[])
{
	char fn[25];
	chdir("Accounts_Storage_Folder");
	chdir(un);
	SetColor(9);
	printf("\n\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(14);
	printf("\n\t\t\tDELETE FILE PERMANENTLY\n");
	SetColor(9);
DFP:    printf("Enter the File Name to be Deleted Permanently:");
	SetColor(13);
	scanf("%s",fn);
	
		int del=remove(fn);
              if(!del){SetColor(14);
                   printf("FILE IS DELETED PERMANATLY");}
              else 
                 { SetColor(14);
                     printf("\nFILE NOT DELETED");
		     SetColor(13);
		     printf("\n\n\tPRESS 1 TO RENTER THE FILE NAME \nPRESS 2 FOR MENU\n");
                     int k;
		     SetColor(10);
                      scanf("%d",&k);
                     if(k==1)
			goto DFP;
                              }
	
	    SetColor(13);
	   chdir("..");
         printf("\n REDIRECTORING TO MENU");
          menu(un,pwd);
}

void retrieve_file(char un[],char pwd[])
{
	SetColor(13);
	printf("\n\t\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	char path[100],path2[100];
	
	chdir("..");
	chdir("..");
	getcwd(path,sizeof(path));
	strcpy(path2,path);
RF:	SetColor(11);
	char file_name[25];
	printf("Enter the filename:");
	SetColor(13);
	scanf("%s",file_name);
	strcat(path,"\\Accounts_Storage_Folder\\");
	strcat(path2,"\\Deleted_Files_Folder\\");
	strcat(path,un);
	strcat(path2,un);
	strcat(path,"\\");
	strcat(path2,"\\");
	strcat(path,file_name);
	strcat(path2,file_name);
	FILE *fp,*dp;
	char c;
//	fp=fopen(path2,"r");
	FILE *cp;int key;
	cp=fopen(path,"r");
	if(cp!=NULL)
	{
		SetColor(12);
		printf("\n\nFILE ALREADY EXISITS");
		SetColor(10);
		printf("\n\nPRESS 1 TO REENTER THE FILE NAME\n");
		scanf("%d",&key);
		if(key==1)
			goto RF;
		else
			menu(un,pwd);
	}
	else
	{
	fclose(cp);
	fp=fopen(path2,"r");
	dp=fopen(path,"w");
	char str[1000];
	while(fgets(str,1000,fp)!=NULL)
		fputs(str,dp);
	fclose(fp);
	fclose(dp);

        menu(un,pwd);
	}

	
}

void write_date(char un[],char p[])
{
	chdir("Accounts_Storage_Folder");
	chdir(un);
	FILE *fp;
	fp=fopen("date.dat","a+");
	char path[100];
	getcwd(path,sizeof(path));
	strcat(path,"\\date.dat");
	
	time_t t;
	time(&t);
	dateu=(struct date *)malloc(sizeof(struct date));
	strcpy(dateu->d,ctime(&t));
	fwrite(dateu,sizeof(struct date),1,fp);
	fclose(fp);
	
	menu(un,p);
}
void view_history(char un[],char pwd[])
{
	SetColor(14);
	printf("\nPRESS ENTER\n");
	if(getch()==13)
		clearing();
	chdir("Accounts_Storage_Folder");
	chdir(un);
	FILE *fp;
	fp=fopen("date.dat","r");
	if(fp==NULL)
		printf("not found");
	dateu=(struct date *)malloc(sizeof(struct date));
	SetColor(13);
	printf("\n\t\t\tTHE HISTORY IS\n");
	while(fread(dateu,sizeof(struct date),1,fp))
	{
		SetColor(9);
		printf("\n%s",dateu->d);
	}
	fclose(fp);
	menu(un,pwd);
}


void unregister(char un[],char pwd[])
{
	SetColor(9);
	printf("\n\t\tPRESS ENTER\n");
	if(getch()==13)
		clearing();
	SetColor(12);
	printf("\n\t\t\t\t\tUNREGISTER\n");
	SetColor(12);
	printf("\n\tNote:If you unregister the whole data will be lost\n");
	SetColor(11);
	printf("If you are sure to unregister enter 1 else you will be redirected to Menu: ");
	int d;
	SetColor(13);
	scanf("%d",&d);
	if(d==1)
	{
		chdir("..");
		chdir("..");
		char s[100];
		FILE *fp,*fp1;
		fp=fopen("log_re.dat","r+");
		user=(struct details *)malloc(sizeof(struct details));
		while(!feof(fp))
		{
			fread(user,sizeof(struct details),1,fp);
			if(strcmp(user->username,un)==0)
			{        his(user->username,pwd);
				strcat(user->username,"abcdefghijklmnopqr");
			
			
				fseek(fp,-(long)sizeof(struct details),1);
				fwrite(user,sizeof(struct details),1,fp);
				break;
			}
			

		}
		fclose(fp);
		SetColor(10);
		printf("\n\t\tUNREGISTERED SUCCESSFULLY\n");
		SetColor(14);
		printf("\n\t\tEXITING FROM THE APPLICATION!!!!!!!!\n");
		
		exit(0);
	}
	else
	{
		menu(un,pwd);
	}
}
void his(char un[],char pwd[])
{
    
	char path[100];

getcwd(path,sizeof(path));

//printf("%s",path);

    DIR *theFolder = opendir(path);
    struct dirent *next_file;
    char filepath[256];

    while ( (next_file = readdir(theFolder)) != NULL )
    {
        
        sprintf(filepath, "%s/%s", path, next_file->d_name);
        remove(filepath);
    }
 
   chdir("..");
int cheack;
   

 
   cheack = rmdir(path);
   if (!cheack)
      printf("Directory deleted\n");
   else
   {  
            printf("Unable to remove directory\n");
          
           
   }
}




	






	




