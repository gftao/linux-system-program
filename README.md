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
            
            5 ioctl(int d, int request, ...) 获取或设置文件本身特有属性，命令有对应的设备文件驱动命令
            /*d 某设备的文件描述符*/
            #include <stdio.h>
            #include <stdlib.h>
            #include <unistd.h>
            #incldue <sys/iocntl.h>
            
            int main(void)
            {
            	struct winsize size;
            	if(isatty(STDOUT_FILENO) == 0)
            		exit(1);
            	if(iocntl(STDOUT_FILENO, TIOCGWINSZ, &size) < 0){ /* TIOCGWINSZ 有驱动层LＣD_read,提供的命令。*/
            		perror("iocntl TIOCGWINSZ error);
            		exit(1);
            	}
            	printf("%d rows, %d columns\n"　size.ws_row, size.ws_col);
            	
            	return 0;
            }
		
		
			
			5.1 ioctl 数码相框应用
			
			#include <stdio.h>
			#include <string.h>
			#include <sys/ioctl.h>
			#include <errno.h>
			#include <sys/mman.h>
			#include <linux/fb.h>
			#include <fcntl.h>
			#include <unistd.h>
			
			int main(void)
			{
				int fd;
				if(fd = open("/dev/fb0", O_RDWR)) < 0){
					perror("open");
					return -1;
				}
				struct fb_var_screeninfo fb_var;
				if(ioctl(fd, FBIOGET_VSCREENINFO, &fb_var) < 0){
					perror("iocntl");
					return -1;
				}
				printf("width:%d\thign:%d\tbpp:%d\n", fb_var.xres, fb_var.yres, fb_var.bits_per_pixel);
				fclose(fd); 
			
				return 0;
			}




