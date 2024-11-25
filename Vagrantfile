Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.network "private_network", type: "dhcp"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get upgrade -y

    sudo apt-get install apt-transport-https ca-certificates curl software-properties-common wget -y

    # GPG oficial do Docker
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    #repositório do Docker
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io -y

    sudo systemctl enable docker
    sudo systemctl start docker
    sudo docker --version

    # diretório para o conteúdo do site G1
    mkdir -p /tmp/g1_globo

    wget -r -l1 --no-parent --no-check-certificate --convert-links --timestamping -P /tmp/g1_globo https://g1.globo.com/go/goias/


    #Dúvida: pq para acessar o serviço do docker (o site hospedado no container), uso o ip da vm, não do container, que é o responsavel por hospedar o website
    # tem relação com esse mapeamento de porta: -p 8080:80??

    sudo docker rm -f meu_servidor_apache

    sudo docker run -d -p 8080:80 --name meu_servidor_apache \
      -v /tmp/g1_globo/g1.globo.com/go/goias:/usr/local/apache2/htdocs \
      httpd
  SHELL
end
