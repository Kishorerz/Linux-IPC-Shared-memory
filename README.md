# Linux-IPC-Shared-memory
int main()
{
   int running =1;
   void *shared_memory = (void *)0; 
   struct shared_use_st *shared_stuff; 
   char buffer[BUFSIZ];
   int shmid;
   shmid	=shmget(	(key_t)1234,	sizeof(struct shared_use_st), 0666 | IPC_CREAT);
   printf("Shared memort id = %d \n",shmid);
   if (shmid == -1)
   {
      fprintf(stderr, "shmget failed\n"); exit(EXIT_FAILURE);
   }
   shared_memory=shmat(shmid, (void *)0, 0);
   if (shared_memory == (void *)-1)
   {
      fprintf(stderr,	"shmat	failed\n"); exit(EXIT_FAILURE);
   }
   printf("Memory Attached at %x\n", (int) shared_memory); 
   shared_stuff = (struct shared_use_st *)shared_memory; 
   while(running)
   {
      while(shared_stuff->written_by_you== 1)   
      {
         sleep(1);
         printf("waiting for client.	\n");
      }
      printf("Enter Some Text: "); fgets (buffer, BUFSIZ, stdin);
      strncpy(shared_stuff->some_text, buffer, TEXT_SZ);
      shared_stuff->written_by_you = 1;
      if(strncmp(buffer, "end", 3) == 0)
      {
         running = 0;
      }
   }
   if (shmdt(shared_memory) == -1)
   {
      fprintf(stderr, "shmdt failed\n"); exit(EXIT_FAILURE);
   }
   exit(EXIT_SUCCESS);
}

```




## OUTPUT

![expt 6 1](https://github.com/KrishnaPrasad148/Linux-IPC-Shared-memory/assets/147332763/0fe55709-27c1-499a-80cf-64152ba8097d)

![expt 6 2](https://github.com/KrishnaPrasad148/Linux-IPC-Shared-memory/assets/147332763/6e32d87c-5ca1-44e1-9091-282421fe2848)

![expt 6 3](https://github.com/KrishnaPrasad148/Linux-IPC-Shared-memory/assets/147332763/b597ef2c-2856-473e-9ad4-26d782896b48)


# RESULT:
The program is executed successfully.
