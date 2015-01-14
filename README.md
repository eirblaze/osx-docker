# osx-docker

docker container on vagrant.

from http://postd.cc/vagrant_with_docker_how_to_set_up_postgres_elasticsearch_and_redis_on_mac_os_x/

1. change DOCKER_OPTS in Vagrantfile.proxy

   because of AUFS probrem?(https://github.com/hw-cookbooks/postgresql/issues/156)
   
2. delete "redis" and "elasticsearch" container.

