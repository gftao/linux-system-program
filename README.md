linux-system-program
====================

linux study 2013/9/2

* chapter-1

            * open.c
            #include <unstd.h>
            #include <sys/stat.h>
            #include <fcnt1.h>
            #include <stdio.h>

            int main(void)
            {
               int fd;
               fd = open("abc", O_CREAT, 0777);
               printf("%d\n", fd);
  
               return 0;
             }  


     * read.c
         #include <unstd.h>
         #include <sys.stat.h>
         #include <fcntl.h>
         #include <stdio.h>

         int main(int argc, char *argv[])
         {
             return 0;
         }
  
