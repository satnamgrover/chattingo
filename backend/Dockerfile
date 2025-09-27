FROM maven:3.9.9-eclipse-temurin-17 AS build

WORKDIR /app

# Copy Maven wrapper files first
COPY mvnw .
COPY .mvn .mvn

# Make sure wrapper is executable
RUN chmod +x mvnw

# Copy pom.xml for dependency caching
COPY pom.xml .

# Optional: Download dependencies (safe fallback: skip if it fails)
RUN ./mvnw dependency:go-offline -B || true

# Copy source code
COPY . .

RUN mvn clean package -DskipTests

# ---- Runtime image ----
FROM eclipse-temurin:17-jdk

WORKDIR /app
COPY --from=build /app/target/*.jar app.jar

# Environment variables (can be overridden at runtime)
ENV JWT_SECRET="z9iomEpv/QtMfd2HTXnLzGt/7+R7I38m9k0T4L7GJa0=" \
	SPRING_DATASOURCE_URL="jdbc:mysql://mysql:3306/chattingo_db?createDatabaseIfNotExist=true" \
	SPRING_DATASOURCE_USERNAME="root" \
	SPRING_DATASOURCE_PASSWORD="test@123" \
	CORS_ALLOWED_ORIGINS="http://52.58.3.148:3000,http://localhost:3000,http://127.0.0.1:3000,http://localhost:3001" \
	CORS_ALLOWED_METHODS="GET,POST,PUT,DELETE,OPTIONS,PATCH" \
	CORS_ALLOWED_HEADERS="*" \
	SPRING_PROFILES_ACTIVE="development" \
	SERVER_PORT="8080"

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
