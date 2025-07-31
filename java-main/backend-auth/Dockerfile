# Giai đoạn 1: Build ứng dụng Spring Boot với Maven
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy toàn bộ mã nguồn và build
COPY . .
RUN mvn clean package -DskipTests

# Giai đoạn 2: Tạo image nhỏ gọn chỉ chứa JAR
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Copy file JAR từ giai đoạn build
COPY --from=build /app/target/*.jar app.jar

# Mở cổng ứng dụng Spring Boot
EXPOSE 8181

# Lệnh chạy ứng dụng
ENTRYPOINT ["java", "-jar", "app.jar"]
