
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
		-subj "/CN=my-receptor-node1" \
		-reqexts v3_req \
		-config config_openssl/n1.conf
	openssl req \
		-nodes \
		-newkey rsa:4096 \
		-out ca/tls2.csr \
		-keyout ca/tls2.key \
		-subj "/CN=my-receptor-node2" \
		-reqexts v3_req \
		-config config_openssl/n2.conf

sign_tls:
	openssl x509 \
		-req \
		-sha256 \
		-days 365 \
		-in ca/tls.csr \
		-CAcreateserial \
		-CA ca/root-ca.crt \
	    -CAkey ca/root-ca.key \
		-out ca/tls.crt \
		-extensions v3_req \
		-extfile config_openssl/sign.conf
	openssl x509 \
		-req \
		-sha256 \
		-days 365 \
		-in ca/tls2.csr \
		-CAcreateserial \
		-CA ca/root-ca.crt \
	    -CAkey ca/root-ca.key \
		-out ca/tls2.crt \
		-extfile config_openssl/n2.conf \
		-extensions v3_req

verify:
	openssl verify -verbose -CAfile ca/root-ca.crt ca/tls.crt
	openssl verify -verbose -CAfile ca/root-ca.crt ca/tls2.crt

n1:
	receptor -c config_nodes/n1.yml

n2:
	receptor -c config_nodes/n2.yml
	#GODEBUG=x509ignoreCN=0 receptor -c config_nodes/n2.yml

clean:
	rm -rf ca
	rm -f n1.sock n1.sock.lock
