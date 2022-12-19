#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <unistd.h>


void generate_random_string(char* str,int len){
    int i;
    for(i=0;i<len;i++){
        str[i] = 'a' + rand()%26;
    }
    str[i] = '\0';
}


int main(int argc, char const *argv[])
{
    int fd1;
    char *fifo1 = "/tmp/myfifo";
    mkfifo(fifo1, 0666);

    char* arr[50];
    char str[100];
    char buffer[100];
    for(int i=0;i<50;i++){
        arr[i] = (char*)malloc(sizeof(char)*10);
        generate_random_string(arr[i],9);
    }
    int i=0;
    while(1){
        fd1=open(fifo1,O_WRONLY);
        buffer[0] = '\0';
        for(int j=0;j<5;j++){
            if(i+j>=50)
                break;
            sprintf(str, "%d %s ",i+j,arr[i+j]);
            strcat(buffer,str);
        }
        printf("message sent: %s\n",buffer);
        write(fd1,buffer,strlen(buffer)+1);
        close(fd1);
        
        fd1=open(fifo1,O_RDONLY);
        read(fd1,buffer,100);
        printf("ID recieved: %s\n",buffer);
        i=atoi(buffer)+1;
        if(i>=50)
            break;
        close(fd1);
    }

    return 0;
}
