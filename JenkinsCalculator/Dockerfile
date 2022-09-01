from openjdk:8

COPY /var/lib/jenkins/workspace/CalculatorPipe/JenkinsCalculator/src/main/java/*.java /

RUN javac /Calculator.java

ENTRYPOINT ["java"]