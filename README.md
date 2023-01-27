# ut-project3
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define MAX 100
struct User //;The users and Their information
{
  char *username;
  char *password;
  struct Post *post;
  struct User *next;
} User;

struct Post // The posts of the users
{
  int id;
  int like;
  char *content;
  struct Post *next;
} Post;

void signup(char *, char *, struct User **);
struct User *like(char *b, struct User *);
struct User *login(char *, char *, struct User *);
struct Post *posting(char *, struct User *, int);

int main()
{
  int id = 0;
  int post_id = 0;
  //int post_id_old =0;
  char sig[7] = "signup";
  char log[6] = "login";
  char pos[5] = "post";
  char lik[7] = "like";
  char lou[9] = "logout\n";
  char del[7] = "delete";
  char inf[6] = "info\n";
  char exi[6] = "exit\n";
  char space[1] = " ";
  struct User *user_list = NULL;
  struct User *current_user = NULL;
  struct User *last_user=NULL;
  struct User *like_user = NULL;

  int choice, co, counter = 0, last=0;
  char *inp = NULL;
  char *inlet = NULL;


  while (1)
  {

    printf("Enter a command\n");
    getline(&inp, &co, stdin);// input of the command
    //co = strlen(inp);

    char *comm = strtok(inp, " ");// Input command untile the space
    char *c = strtok(NULL, "");// Restore of the command untile the end
    char *b = strtok(c, " ");//The second part of command after the space untile the second space
    char *d = strtok(NULL, "");// The restore of the command untile the end

////////////////////////////////////////////////////////////////////////////////////////////////////// Signup command 
    if (strcmp(comm, sig) == 0)// Investigation if the command is signup
    {
      int a=0;
       struct User *head = user_list;
             while(head !=NULL){
      if(strcmp(head->username, b) == 0){printf("The username is avaleable please enter the new one\n"); a=1; break;}
       else
       {head=head->next;}
       }
       if(a==0){
      signup(b, d, &user_list);}
    }
//////////////////////////////////////////////////////////////////////////////////////////////////// Login command 
    else if (strcmp(comm, log) == 0)// Investigation if the command is login
    {
      current_user = NULL;
      current_user = login(b, d, user_list);// The user who is loged in
      while (current_user)// Untile the user is loge in
      {
        printf("Dear %s You are login , please Enter a command\n", b);
/////////////////////////////////////////////////////////////////////////////////The functions of the loged in user

        getline(&inlet, &counter, stdin);// enter the command after the login (post - like - logout - info)

       // counter = strlen(inlet);
        char *order = strtok(inlet, " ");//Input command untile the space
/////////////////////////////////////////////////////////////////////////////////////posting
        if (strcmp(order, pos) == 0)// if the enter command is post
        {
           struct Post *tt=current_user->post;

         if (last_user==NULL){post_id=1;}
         // post_id++;
       else if(last_user==NULL && post_id>0){post_id++;}
    else if(strcmp(current_user->username, last_user->username)==0){post_id ++;}
  else {
  
  post_id=1;
  if (tt){if (current_user->post->id >0 && current_user->post->id <200){post_id=current_user->post->id +1;}}

  }
 current_user->post=tt;
          char *order1 = strtok(NULL, "");// Post content which is written after the pace after the post command

          struct Post *post = posting(order1, current_user, post_id);
          last_user=current_user;
        }
/////////////////////////////////////////////////////////////////////////////////////Like
        else if (strcmp(order, lik) == 0)// if the enter command is like
        {
          char *order1 = strtok(NULL, "");//Input command untile the space
          char *order2 = strtok(order1, " ");//username to post
          char *order3 = strtok(NULL, "");// ID umber of the selected post
          int x;

          x = atoi(order3);// convert the post ID to intiger type
          struct User *head = user_list;// All of the user
          while (head != NULL)
          {
            if (strcmp(head->username, order2) == 0)// If the username of the user is the same of the input username
            {
              like_user = head;
            
              head = NULL;
              //break;
            }
            else
            {
              head = head->next;//Go to the next user
            }
          }
         struct Post *temp= like_user->post;
          while (like_user->post)
          {
            if (strcmp(current_user->username, like_user->username) == 0){printf("\nYou can not like your post\n");break;}
            if (like_user->post->id == x)//investigation of the ID number of the selected user
            {
              
             
             like_user->post->like++;// Add to the like number of the selected post ID of the selected user
             
              printf("\nThe post ID is: %d\nlike of this post is: %d \npost content is: %s", like_user->post->id, like_user->post->like, like_user->post->content);
               
                like_user->post= temp;          
               break;
            }

            else
            {
              like_user->post = like_user->post->next;/////// go to the next post of the selected user
              if(like_user->post->id==1){ like_user->post= temp; printf("\nThe selected post is not avaleabel\n"); break;}
          
            }
          }
          
        }
 //////////////////////////////////////////////////////////////////////////////////////////Delete  a post
     
        else if (strcmp(order, del) == 0)
        {
          char *order1 = strtok(NULL, "");//The ID of the selected post for delete
          int dl_id = atoi(order1);/// Converting the ID of the selected post for delete to intider type
          if (current_user->post->id < dl_id){printf("Selected post is not exist\n");}//If the selected post is not avaleable
          struct Post **delete_head = &(current_user->post);
          struct Post *temp;
                    
          if ((*delete_head) != NULL)
          {
            if ((*delete_head)->id == dl_id)///If the post ID is the selected post ID
            {
              temp = *delete_head;
              *delete_head = (*delete_head)->next;
             
             free(temp);
             // temp=NULL;
            }
            else
            {
              struct Post *cursor = *delete_head;
              while (cursor->next != NULL)
              {
                if (cursor->next->id == dl_id)
                {
                  temp = cursor->next;
                  cursor->next = cursor->next->next;
                  free(temp);
                // temp=NULL;
                  break;
                }
                else
                {
                  cursor = cursor->next;
                 }
                
              }
            }
          }
/////////////////////////////////////////////////////////////////////Show the information of the loged in user      
        }
        else if (strcmp(order, inf) == 0)// If the command is getting the information
        {
          
          printf("username: %s\n", current_user->username);
          printf("password: %s\n", current_user->password);
          
          struct Post *temmp= current_user->post;
          while (current_user->post !=NULL)
          {
            if(current_user->post->id>5000){current_user->post=temmp;break;}
            printf("post_id: %d\n", current_user->post->id);
            printf("like: %d\n", current_user->post->like);
            printf("Post Content: %s\n", current_user->post->content);
           if(current_user->post->id==1){ current_user->post=temmp; break;}
           if(current_user->post->id>5000){current_user->post=temmp;break;}
            current_user->post = current_user->post->next;//Go to the previuse post
            
            
          }
        }
 ////////////////////////////////////////////////////////////////////////Loging out       
        else if (strcmp(order, lou) == 0)
        {
         // last_user=current_user;
          last=1;
          //post_id=0;
          // printf("%s", order);
          printf("\nYou are successfully logout\n");
          current_user = NULL;
        }
///////////////////////////////////////////////////////////////////////////If the loged in user Command is incorrect        
       
         else
        {
          printf("Please enter a correct command\n");
        }
      }
    }
////////////////////////////////////////////////////////////////////////////// Exit the programm  
     else if (strcmp(comm, exi) == 0)
     { struct User *user = (struct User *)malloc(sizeof(struct User));
    free(user);
     free(user->username);
     free(user->password);
     free(user->post);
     struct Post *post = (struct Post *)malloc(sizeof(struct Post));
     free(post);
     free(post->content);
              
      break;}
////////////////////////////////////////////////////////////////////////////If the first command is not login or signup     
    else
    {
      printf("Please enter a correct command\n");
    }
  }
}
///////////////////////////////////////////////////////////////////Signup functio
void signup(char *b, char *d, struct User **user_list)
{
  int count1 = 0, count2 = 0;

  count1 = strlen(b);// The length of the username
  count2 = strlen(d);// The length of the password
  struct User *user = (struct User *)malloc(sizeof(struct User));// Alocate the memory to the user struct
  user->username = malloc(sizeof(char) * count1);//Alocate the memory to the username as the username length
  strcpy(user->username, b);
  user->password = malloc(sizeof(char) * count2);// //Alocate the memory to the username as the username passwor
  strcpy(user->password, d);
  user->next = (*user_list);
  (*user_list) = user;
}
////////////////////////////////////////////////////////////////////////Login function
struct User *login(char *b, char *d, struct User *user_list)
{
  register int a = '\0';
  struct User *head = user_list;
  while (head != NULL)
  {
    if (strcmp(head->username, b) == 0 && strcmp(head->password, d) == 0)// If the username and password is the same of the entered one
    {
      return head;// find the selected user for login
    }
    else
    {
      head = head->next;//go to the next user
    }
  }
  printf("The user not found\n");///if the username and password is not found
}
/////////////////////////////////////////////////////////////////////////////////Posting function
struct Post *posting(char *content, struct User *current_user, int post_id)
{
  struct Post *post = (struct Post *)malloc(sizeof(struct Post));//Alocate the memory to the post Structure
   post->id=post_id;//Alocate the post ID
  post->like = 0;
  post->content = malloc(sizeof(char) * strlen(content));//Alocate the memory to the enterd post as the its length
  strcpy(post->content, content);
  post->next = current_user->post;
  current_user->post = post;
  return post;
}

