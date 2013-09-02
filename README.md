linux-system-program
====================

linux study 2013/9/2

* chapter-1

            1 open.c
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


           2 read.c
           #include <unstd.h>
           #include <sys.stat.h>
           #include <fcntl.h>
           #include <stdio.h>
           
           int main(int argc, char *argv[])
           {
             int fd;
             int fd1;
             int len;
             
             fd = open(argv[1], O_READONLY);
             fd1 = open(argc[2], O_WRONLY|O_CREAT, 0644);
             if(fd < 0 || fd1 < 0){
               perror("open");
               exit(-1);
             }
             len = read(fd, buf, sizeof(buf));
             write(fd1, buf, len);
             
             return 0;
           }
  
