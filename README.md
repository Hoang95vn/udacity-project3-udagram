docker-compose config
# Run from the directory where you have the compose file present
docker-compose down
# To delete all dangling images
docker image prune --all

docker-compose -f docker-compose-build.yaml build --parallel

docker-compose up