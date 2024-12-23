# WebGoat: A deliberately insecure Web Application for security training

WebGoat is an intentionally vulnerable web application designed to teach web application security concepts. It provides a safe environment for developers and security professionals to learn and practice various security techniques.

## Project Description

WebGoat is an Open Web Application Security Project (OWASP) utility that offers hands-on learning experiences for common web application vulnerabilities. It includes a series of lessons covering different security topics, such as SQL injection, cross-site scripting (XSS), authentication flaws, and more.

Key features:
- Interactive lessons with practical exercises
- Realistic scenarios simulating common security vulnerabilities
- Built-in scoring system to track progress
- Support for multiple languages
- Containerized deployment for easy setup

WebGoat is designed to be used in educational settings, security training programs, and for individual learning. It allows users to explore and understand security concepts in a controlled environment without risking real systems.

## Repository Structure

```
.
├── src
│   ├── it
│   │   └── java
│   │       └── org/owasp/webgoat
│   │           ├── CSRFIntegrationTest.java
│   │           ├── IDORIntegrationTest.java
│   │           ├── IntegrationTest.java
│   │           └── JWTLessonIntegrationTest.java
│   └── main
│       ├── java
│       │   └── org/owasp/webgoat/container
│       │       ├── DatabaseConfiguration.java
│       │       ├── MvcConfiguration.java
│       │       └── ...
│       └── resources
├── .mvn
│   └── wrapper
│       └── MavenWrapperDownloader.java
├── config
│   ├── checkstyle
│   ├── dependency-check
│   └── desktop
├── docs
├── robot
├── Dockerfile
├── pom.xml
└── README.md
```

Key files:
- `pom.xml`: Maven project configuration file
- `Dockerfile`: Docker image definition for WebGoat
- `src/main/java/org/owasp/webgoat/container/`: Core WebGoat application code
- `src/it/java/org/owasp/webgoat/`: Integration tests

## Usage Instructions

### Installation

Prerequisites:
- Java 23 or later
- Maven 3.6 or later
- Docker (optional, for containerized deployment)

To build WebGoat:

```bash
mvn clean install
```

### Running WebGoat

To run WebGoat locally:

```bash
java -jar webgoat-server/target/webgoat-server-<version>.jar
```

To run WebGoat using Docker:

```bash
docker run -p 8080:8080 -p 9090:9090 webgoat/webgoat
```

Access WebGoat at `http://localhost:8080/WebGoat`

### Getting Started

1. Register a new account on the WebGoat application.
2. Navigate through the lessons in the left sidebar.
3. Complete the interactive exercises for each lesson.
4. Use the hints provided if you get stuck on a particular challenge.

### Configuration

WebGoat can be configured using environment variables or by modifying the `application.properties` file. Some important configuration options include:

- `WEBGOAT_PORT`: The port WebGoat runs on (default: 8080)
- `WEBWOLF_PORT`: The port WebWolf (a companion application) runs on (default: 9090)
- `WEBGOAT_SSLENABLED`: Enable/disable SSL (default: false)

### Integration

WebGoat can be integrated into your security training programs or development workflows. Consider the following:

- Use WebGoat as part of your developer onboarding process to introduce security concepts.
- Incorporate WebGoat exercises into your continuous integration pipeline to regularly test developers' security knowledge.
- Extend WebGoat with custom lessons specific to your organization's security requirements.

### Testing & Quality

To run the integration tests:

```bash
mvn verify
```

### Troubleshooting

Common issues and solutions:

1. Problem: Unable to start WebGoat
   - Error message: "Port 8080 already in use"
   - Solution: Change the port using the `server.port` property or `WEBGOAT_PORT` environment variable

2. Problem: Lessons not loading
   - Error message: "Error loading lesson content"
   - Diagnostic steps:
     1. Check the application logs for specific error messages
     2. Verify that all required dependencies are installed
     3. Ensure you have the latest version of WebGoat
   - Solution: If the issue persists, try clearing your browser cache or reinstalling WebGoat

3. Problem: Integration tests failing
   - Error message: "Test XXX failed due to timeout"
   - Debugging:
     1. Increase the test timeout in the `pom.xml` file
     2. Run tests with verbose logging: `mvn verify -X`
   - Solution: Analyze the detailed log output to identify the root cause of the test failure

For more detailed troubleshooting, enable debug mode by adding the following VM option:
```
-Dlogging.level.org.owasp.webgoat=DEBUG
```

Performance optimization:
- Monitor CPU and memory usage during WebGoat sessions
- Consider increasing the Java heap size if running large-scale training sessions
- Use JProfiler or VisualVM to identify performance bottlenecks in the application

## Data Flow

1. User sends a request to WebGoat through their web browser
2. The request is received by the Spring MVC framework (MvcConfiguration.java)
3. The appropriate controller handles the request based on the URL mapping
4. If the request is for a lesson, the LessonTemplateResolver loads the lesson content
5. The controller processes the request, potentially interacting with the database (DatabaseConfiguration.java)
6. The response is generated, often using Thymeleaf templates
7. The response is sent back to the user's browser

```
[Browser] <-> [Spring MVC] <-> [Controller] <-> [LessonTemplateResolver]
                                  ^
                                  |
                                  v
                          [DatabaseConfiguration]
```

Note: The data flow may vary depending on the specific lesson or functionality being accessed.

## Deployment

Prerequisites:
- Docker
- Docker Compose (optional, for multi-container deployment)

Deployment steps:
1. Build the Docker image:
   ```
   docker build -t webgoat .
   ```
2. Run the WebGoat container:
   ```
   docker run -p 8080:8080 -p 9090:9090 webgoat
   ```
3. Access WebGoat at `http://localhost:8080/WebGoat`

For production deployments, consider:
- Using a reverse proxy (e.g., Nginx) for SSL termination
- Implementing proper access controls and authentication mechanisms
- Regularly updating WebGoat to the latest version

## Infrastructure

The WebGoat application is primarily built using Java and Spring Boot. Key infrastructure components include:

- Spring Boot (Application framework)
  - MvcConfiguration: Configures Spring MVC for handling web requests
  - DatabaseConfiguration: Sets up the database connection and Flyway migrations
- Thymeleaf (Templating engine)
- HSQLDB (In-memory database)
- Flyway (Database migration tool)
- Docker (Containerization)

The Dockerfile defines the following:
- Base image: eclipse-temurin:23-jdk-noble
- Exposed ports: 8080 and 9090
- Entry point: Java command to run the WebGoat JAR file
- Health check: Curl command to verify the application is running

The application is designed to run in a single container, with the option to use WebWolf (a companion application) in a separate container if needed.