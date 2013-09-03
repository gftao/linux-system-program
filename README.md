linux-system-program
====================

linux study 2013/9/2

* chapter-1
* 


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
  
            3 lseek.c
            #include <sys/types.h>
            #include <sys/stat.h>
            #include <unistd.h>
            #include <fcntl.h>
            #include <stdio.h>
            #include <stdlib.h>
            
            int main(void)
            {
                        int fd;
                        int size;
                        char buf[4096] = "hello";
                        char str[4096] ;
                        fd = open("abc", O_CREAT|O_RDWR|O_TRUNC, 0777);
                        if(fd == -1){
                                    perror("open");
                                    exit(-1);
                        }
                        len = write(fd, buf, strlen(buf));
                        lseek(fd, 0 - len, SEEK_CUR);
                        read(fd, str, len);
                        str[len] = '\0';
                        write(STDOUT_FILENO, str, len);
                        
                        return 0;
                          
            }
            ## open("abc", O_RDWR|O_APPEND, 0777)中的 O_APPEND参数 对 lseek("abc", 100, SEEK_END) 拓展空间有干扰！；



            4 fcntl(fd, F_DUPFD, flags)
            /*flags 如果已用，如flags = 0，fcntl返回一个最小的未用的一个文件描述符; 如果flags没用，fcntl返回flags*/
            #include <unitd.h>
            #include <fcntl.h>
            #include <stdio.h>
            
            int main(void)
            {
            	int fd, flags;
            	
            	fd = open("somefile" O_RDWR|O_CREAT, 0644);
            	flags = fcntl(fd, F_DUPFD, 0);
            	if(flags < 0){
            		perror("fcntl");
            		exit(1);
            	}
            	if(fd < 0){
            		perror("open");
            		exit(1);
            	}
            	printf("%d\n", flags);
            	write(fd, "hello ", 6);
            	write(flags, "world!\n", 7);
            	close(fd);
            	close(flags);
            	
            	return 0;
	        }
            





