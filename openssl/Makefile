
all: root_ca_init root_ca sign_ca tls sign_tls verify

root_ca_init:
	mkdir -p ca

root_ca: root_ca_init
	openssl req \
		-nodes \
		-newkey rsa:4096 \
		-subj "/CN=ansible.com" \
		-out ca/root-ca.csr \
		-keyout ca/root-ca.key
sign_ca:
	openssl x509 \
		-req \
		-sha256 \
		-days 365 \
		-in ca/root-ca.csr \
		-signkey ca/root-ca.key \
		-out ca/root-ca.crt

tls:
	openssl req \
		-nodes \
		-newkey rsa:4096 \
		-out ca/tls.csr \
		-keyout ca/tls.key \
		-subj "/CN=my-receptor-node1"
		#-addext "subjectAltName = DNS:my-receptor-node1.ansible.com"
	openssl req \
		-nodes \
		-newkey rsa:4096 \
		-out ca/tls2.csr \
		-keyout ca/tls2.key \
		-subj "/CN=my-receptor-node2"
		#-addext "subjectAltName = DNS:my-receptor-node2.ansible.com"

sign_tls:
	openssl x509 \
		-req \
		-sha256 \
		-days 365 \
		-in ca/tls.csr \
		-CAcreateserial \
		-CA ca/root-ca.crt \
	    -CAkey ca/root-ca.key \
		-out ca/tls.crt
	openssl x509 \
		-req \
		-sha256 \
		-days 365 \
		-in ca/tls2.csr \
		-CAcreateserial \
		-CA ca/root-ca.crt \
	    -CAkey ca/root-ca.key \
		-out ca/tls2.crt

verify:
	openssl verify -verbose -CAfile ca/root-ca.crt ca/tls.crt
	openssl verify -verbose -CAfile ca/root-ca.crt ca/tls2.crt

n1:
	receptor -c config_receptor/n1.yml

n2:
	receptor -c config_receptor/n2.yml

clean:
	rm -rf ca
	rm -f n1.sock n1.sock.lock
