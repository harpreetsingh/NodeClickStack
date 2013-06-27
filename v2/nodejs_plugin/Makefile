compile: node.tar.gz
	rm -f /tmp/mynode.zip
	zip /tmp/mynode.zip *

node.tar.gz: 
#	curl -o node.tar.gz http://nodejs.org/dist/v0.10.12/node-v0.10.12-linux-x64.tar.gz
	curl -o node.tar.gz http://nodejs.org/dist/v0.10.12/node-v0.10.12-darwin-x64.tar.gz

publish: compile
	s3cmd put /tmp/mynode.zip -P s3://harpreet-clickstacks.cloudbees

clean:
	rm node.tar.gz
	rm /tmp/mynode.zip
