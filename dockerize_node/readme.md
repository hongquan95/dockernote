docker build -t my-node:v1.0 .
docker run -d --name=my-node-container -p 3000:3000 my-node:v1.0 //Not using volume
docker run -d --name=my-node-container -v $(pwd):/src/app -v /src/app/node_modules -p 3000:3000 my-node:v1.0 //Using volume
