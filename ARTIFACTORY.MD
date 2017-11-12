//See more at https://www.jfrog.com/confluence/display/RTF/Installing+with+Docker
docker pull docker.bintray.io/jfrog/artifactory-oss:latest 
docker volume create --name artifactory5_data
docker run --name artifactory -d -v artifactory5_data:/var/opt/jfrog/artifactory -p 8081:8081 docker.bintray.io/jfrog/artifactory-oss:latest