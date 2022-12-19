#include <unistd.h>
#include <stdio.h>
#include <sys/socket.h>
#include <stdlib.h>
#include <netinet/in.h>
#include <string.h>


#define PORT 8080



void generate_random_string(char* str,int len){
    int i;
    for(i=0;i<len;i++){
        str[i] = 'a' + rand()%26;
    }
    str[i] = '\0';
}


int main(int argc, char const *argv[])
{
    int server_fd, mySocket, valread;
    struct sockaddr_in address;
    int opt = 1;
    int addrlen = sizeof(address);
    char buffer[1024] = {0};

    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    if(server_fd == 0){
        perror("socket failed");
        exit(EXIT_FAILURE);
    }

    if(setsockopt(server_fd,SOL_SOCKET, SO_REUSEADDR | SO_REUSEPORT, &opt,sizeof(opt))){
        perror("setsockopt failed");
        exit(EXIT_FAILURE);
    }
    address.sin_family=AF_INET;
    address.sin_addr.s_addr=INADDR_ANY;
    address.sin_port=htons(PORT);

    if(bind(server_fd, (struct sockaddr *)&address, sizeof(address))<0){
        perror("bind failed");
        exit(EXIT_FAILURE);
    }

    if(listen(server_fd, 3) < 0){
        perror("listen");
        exit(EXIT_FAILURE);
    }

    mySocket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen);
    if(mySocket<0){
        perror("accept");
        exit(EXIT_FAILURE);
    }


    char* arr[50];
    char str[100];
    for(int i=0;i<50;i++){
        arr[i] = (char*)malloc(sizeof(char)*10);
        generate_random_string(arr[i],9);
    }
    int i=0;
    while(1){
        buffer[0] = '\0';
        printf("\n--------------------\n");
        for(int j=0;j<5;j++){
            if(i+j>=50)
                break;
            sprintf(str, "%d %s ",i+j,arr[i+j]);
            strcat(buffer,str);
        }
        printf("message sent: %s\n",buffer);
        send(mySocket, buffer, strlen(buffer), 0);
        
        buffer[2] = '\0';
        valread = read( mySocket , buffer, 3);
        printf("ID recieved: %s\n",buffer);
        i=atoi(buffer)+1;
        if(i>=50)
            break;
    }

    return 0;
}
