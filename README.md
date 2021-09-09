# Comandos que podem ser necessários
mvn spring-boot:run -Dspring-boot.run.profiles={profile_name}

mvn clean spring-boot:run -Dspring-boot.run.profiles=dev 
mvn clean spring-boot:run -Dspring-boot.run.profiles=prod 


java -jar -Dspring.profiles.active=prod forum.jar

Para utilizar banco diferente do banco em memoria para rodar os  testes, utilizar o @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.replace.NONE)

EXPORT FORUM_DATABASE_URL=jdbc:h2:mem:alura-forum
EXPORT FORUM_DATABASE_USERNAME=sa
EXPORT FORUM_DATABASE_PASSWORD=
EXPORT FORUM_JWT_SECRET=UmTeXtOQualQUER


java -jar -Dspring.profiles.active=prod -DFORUM_DATABASE_URL=jdbc:h2:mem:alura-forum -DFORUM_DATABASE_USERNAME=sa -DFORUM_DATABASE_PASSWORD= -DFORUM_JWT_SECRET=UmTeXtOQualQUER forum.jar

# gerar um war


1 - alterar o POM.xml acima das tags properties, dependencies
<packaging>war</packaging>

2 - alterar o POM.xml - adicionar a dependencia do spring-boot-starter-tomcat com scope provided

3 - A classe Main deve extender de SpringBootServeletInitializer

4 - override um metodo da extensao o configure 

@Override
protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
    //passando a classe main como parametro
    return builder.sources(ForumApplication.class)
}


gerando uma imagem 

docker build -t alura/forum .

docker image list 
//verificar as imagens geradas..

docker run -p 8080:8080 -e SPRING_PROFILES_ACTIVE='prod' -e FORUM_DATABASE_URL='jdbc:h2:mem:alura-forum' -e FORUM_DATABASE_USERNAME='sa' -e FORUM_DATABASE_PASSWORD='' -e FORUM_JWT_SECRET='UmTeXtOQualQUER' alura/forum



heroku 

heroku container:login
heroku create forum-acelera
heroku git:remote -a forum-acelera
heroku container:push web
heroku container:release web
heroku open