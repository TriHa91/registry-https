# Before install

## add host
sudo nano /etc/host
IP registry.k8s

## Generate self-signed certificates
sudo mkdir -p /home/user/registry/{auth,certs,data}

sudo openssl req -newkey rsa:4096 -nodes -sha256 \
  -keyout /home/user/registry/certs/domain.key \
  -x509 -days 365 -out /home/user/registry/certs/domain.crt \
  -subj "/CN=registry.k8s" \
  -addext "subjectAltName = DNS:registry.k8s"

## Create authentication credentials

sudo apt update
sudo apt install apache2-utils -y

sudo htpasswd -Bbn username password > /home/user/registry/auth/htpasswd
or
sudo sh -c "htpasswd -Bbn username username > /home/user/registry/auth/htpasswd"

## Configure Docker clients to trust your registry

sudo mkdir -p /etc/docker/certs.d/registry.k8s:9443

sudo cp /home/odl_user_1690629/registry/certs/domain.crt /etc/docker/certs.d/registry.k8s:9443/ca.crt

docker login registry.k8s:9443 -u trihn -p trihn

curl -v https://registry.k8s
