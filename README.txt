1. ������ ����
gcc -g -Wall -pthread -c reentrant.c -o reentrant.o 
gcc -g -Wall -pthread -c common.c -o common.o

gcc -g -Wall -pthread -c client.c -o client.o 
gcc -g -Wall -pthread -c server.c -o server.o

gcc -g -Wall -pthread -o client client.o common.o reentrant.o -lssl -lcrypto
gcc -g -Wall -pthread -o server server.o common.o reentrant.o -lssl -lcrypto

2. ������ �����
/usr/bin/openssl req -newkey rsa:1024 -sha1 -keyout rootkey.pem -out rootreq.pem -config root.cnf

/usr/bin/openssl x509 -req -in rootreq.pem -sha1 -extfile root.cnf -extensions certificate_extensions -signkey rootkey.pem -out rootcert.pem

/bin/cat rootcert.pem rootkey.pem > root.pem

/usr/bin/openssl req -newkey rsa:1024 -sha1 -keyout serverCAkey.pem -out serverCAreq.pem -config serverCA.cnf

/usr/bin/openssl x509 -req -in serverCAreq.pem -sha1 -extfile serverCA.cnf -extensions certificate_extensions -CA root.pem -CAkey root.pem -CAcreateserial -out serverCAcert.pem

/bin/cat serverCAcert.pem serverCAkey.pem rootcert.pem > serverCA.pem

/usr/bin/openssl req -newkey rsa:1024 -sha1 -keyout serverkey.pem -out serverreq.pem -config server.cnf -reqexts req_extensions

/usr/bin/openssl x509 -req -in serverreq.pem -sha1 -extfile server.cnf -extensions certificate_extensions -CA serverCA.pem -CAkey serverCA.pem -CAcreateserial -out servercert.pem

/bin/cat servercert.pem serverkey.pem serverCAcert.pem rootcert.pem > server.pem

/usr/bin/openssl req -newkey rsa:1024 -sha1 -keyout clientkey.pem -out clientreq.pem -config client.cnf -reqexts req_extensions

/usr/bin/openssl x509 -req -in clientreq.pem -sha1 -extfile client.cnf -extensions certificate_extensions -CA root.pem -CAkey root.pem -CAcreateserial -out clientcert.pem

/bin/cat clientcert.pem clientkey.pem rootcert.pem > client.pem


2. �������
	1) ./server ���� -> ������ ���鶧 ����ߴ� ��й�ȣ �Է�
	2) ./client ���� (���߽��డ��) -> ������ ���鶧 ����ߴ� ��й�ȣ �Է�
	3) client, server ����� Ctrl + c �� ����

cf) zip���Ͽ� ������ �������� ��й�ȣ�� 130613�Դϴ�.