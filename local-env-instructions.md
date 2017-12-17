### These instuctions assume a local VirtualBox environment
This local environment has been testeed on a Windows 10 machine with 12G RAM. Pre-requites are as follows:
- Latest version of VirtualBox
- Latest version of GitBash
- Latest version of Vagrant
  - Install Vagrant Host Manager plugin by running `vagrant plugin install vagrant-hostmanager` in Git bash. This will update host files on both guest and host machines

##### Follow these instructions sequentially:
1. Open a GitBash shell
2. `git clone https://github.com/shazChaudhry/spring-petclinic.git`
3. `cd spring-petclinic`
4. `vagrant up` _(This will start MySQL and SonarQube)_
5. `vagrant ssh node1`
6. `docker ps -a` _(This is to check MySQL and SonarQube are running)_
  - http://node1:9000 _(username/password admin/admin)_
7. `cd /vagrant`
8. `alias mvn='docker container run -it --rm --name maven -v ~/.m2:/root/.m2 -v /var/run/docker.sock:/var/run/docker.sock -v $PWD:/maven -w /maven maven:alpine mvn'` _(Note that maven local repository will be built at ~/.m2)_
9. `mvn --version` _(This should print Maven version along with Java version)_
10. `mvn clean install`
  - maven surefire plugin is configured to ignore test failures so the build will stop
  - target/classes/git.properties will be created and contains git branch, commit and committer info
11. `mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.4.0.905:sonar -Dsonar.host.url=http://node1:9000`
