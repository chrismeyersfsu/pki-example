receptor := ~/ansible/receptor/receptor

all: init
	$(receptor) --config config_certs/cert_init.yml
	$(receptor) --config config_certs/cert_request.yml
	$(receptor) --config config_certs/cert_sign.yml
	$(receptor) --config config_certs/cert_request2.yml
	$(receptor) --config config_certs/cert_sign2.yml

init:
	mkdir -p certs

verify:
	openssl verify -verbose -CAfile certs/generated_by_receptor_ca_cert certs/generated_by_receptor_node_signed_cert
	openssl verify -verbose -CAfile certs/generated_by_receptor_ca_cert certs/generated_by_receptor_node_signed_cert2

clean:
	rm -rf certs
	rm -f n1.sock n1.sock.lock

n1:
	receptor -c config_nodes/n1.yml

n2:
	receptor -c config_nodes/n2.yml
