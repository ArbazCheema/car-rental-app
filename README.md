# Car Rental App

A web-based car rental comparison system that allows users to search, compare, and rent cars from multiple rental companies including Hertz, Enterprise, Aloma, National Cars, Dollar and Sixt.

## Features

-  Search for cars by name, model, or other attributes
-  Filter cars by price, fuel type, and transmission
-  Compare up to 3 cars side by side
-  View detailed information about each car
-  Direct links to rental company websites
-  Responsive design for desktop and mobile devices
-  Auto-completion and spell checking for search terms
-  Search frequency tracking and popular searches display

## Prerequisites

Before running this project, make sure you have the following installed:

-  Java Development Kit (JDK) 11 or higher
-  Maven (for dependency management)
-  Web browser (Chrome, Firefox, Safari, etc.)
-  Firefox browser (for web crawling functionality)

## Project Structure

The project follows a layered architecture with separation of concerns:

```
src/
└── com/
    └── group4/
        ├── model/              # Data models
        │   ├── Car.java
        │   ├── FuelType.java
        │   └── TransmissionType.java
        ├── service/            # Business logic
        │   ├── WebCrawlerService.java
        │   ├── search/
        │       ├── SearchService.java
        │       ├── SearchFrequencyService.java
        │       ├── WordCompletionService.java
        │       └── SpellCheckingService.java
        ├── repository/         # Data access
        │   └── CarRepository.java
        ├── util/               # Utility classes
        │   ├── CustomPrint.java
        │   └── Utils.java
        ├── web/                # Web-related components
        │   ├── servlet/        # Servlets
        │   │   ├── CarServlet.java
        │   │   ├── ScraperServlet.java
        │   │   └── AdminServlet.java
        │   └── html/           # HTML files
        │       └── index.html
        └── WebServerLauncher.java  # Web server launcher
```

## Implemented Features

1. **Web Crawler**: Crawls car rental websites to gather data.
2. **HTML Parser**: Parses HTML content to extract car information.
3. **Spell Checking**:
   -  Constructs a vocabulary based on existing words.
   -  Provides alternative word suggestions if no results are found.
   -  Uses Levenshtein distance algorithm to compare user input with existing words.
4. **Word Completion**:
   -  Provides auto-completion suggestions for search terms.
   -  Implemented using a Trie data structure for efficient prefix matching.
5. **Frequency Count**:
   -  Shows the number of occurrences of a word in specific URLs.
6. **Search Frequency**:
   -  Tracks and displays words that have been searched before.
   -  Shows the number of times each word has been searched.
7. **Page Ranking**:
   -  Measures the importance of search results based on the number of occurrences.
   -  Keywords repeated more within a web page are ranked higher.
8. **Inverted Indexing**:
   -  Enables efficient searching of content.
9. **Data Validation**:
   -  Uses regular expressions to validate various data formats.

## Installing Dependencies

The project uses Maven for dependency management. To install all required dependencies:

1. Make sure Maven is installed on your system. You can check by running:

   ```bash
   mvn -version
   ```

2. Navigate to the project root directory (where the pom.xml file is located).

3. Run the following command to download and install all dependencies:

   ```bash
   mvn clean install -DskipTests
   ```

   This command will:

   -  Clean any previous build artifacts
   -  Download all required dependencies defined in pom.xml
   -  Build the project
   -  Install the project to your local Maven repository

4. All dependencies will be downloaded to the `target/lib` directory, including:
   -  Jetty server libraries
   -  JSON processing libraries
   -  JSoup HTML parser
   -  Selenium WebDriver for Firefox
   -  OpenTelemetry libraries

If you encounter any dependency-related errors, running this command should resolve them by ensuring all required libraries are properly downloaded.

## How to Run the Project

### Using Maven

The project uses Maven for dependency management and build automation. To run the project:

1. Clone the repository to your local machine
2. Navigate to the project root directory
3. Run the following command:

```bash
mvn clean compile exec:java
```

This will compile the project and start the web server on port 8080.

### Using Java Command

Alternatively, you can run the project using the Java command:

1. Compile the project:

```bash
mvn clean package
```

2. Run the application:

```bash
java -jar target/car-rental-crawler-1.0-SNAPSHOT.jar
```

### Accessing the Application

Once the server is running, open your web browser and navigate to:

```
http://localhost:8080
```

## API Endpoints

The application provides several API endpoints for interacting with the car rental data:

### Car API Endpoints

Base URL: `/api/cars`

| Endpoint   | Method | Description                     | Parameters                                                                                                             |
| ---------- | ------ | ------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `/list`    | GET    | List all cars                   | `page` (default: 1), `pageSize` (default: 10), `sortBy` (optional)                                                     |
| `/search`  | GET    | Search for cars                 | `query` (required), `fuelType` (optional), `transmissionType` (optional), `minPrice` (optional), `maxPrice` (optional) |
| `/suggest` | GET    | Get auto-completion suggestions | `query` (required)                                                                                                     |
| `/popular` | GET    | Get popular search terms        | None                                                                                                                   |

### Scraper API Endpoints

Base URL: `/api/scrape`

| Endpoint | Method | Description                           | Parameters |
| -------- | ------ | ------------------------------------- | ---------- |
| `/`      | GET    | Refresh car data by crawling websites | None       |

### Admin API Endpoints

Base URL: `/api/admin`

| Endpoint        | Method | Description                                   | Parameters |
| --------------- | ------ | --------------------------------------------- | ---------- |
| `/refresh-data` | POST   | Refresh all data and reset search frequencies | None       |

## Example API Usage

### List all cars

```
GET http://localhost:8080/api/cars/list?page=1&pageSize=10
```

### Search for cars

```
GET http://localhost:8080/api/cars/search?query=Toyota&fuelType=PETROL&minPrice=50&maxPrice=200
```

### Get auto-completion suggestions

```
GET http://localhost:8080/api/cars/suggest?query=Toy
```

### Get popular search terms

```
GET http://localhost:8080/api/cars/popular
```

### Refresh car data

```
GET http://localhost:8080/api/scrape/
```

### Refresh all data and reset search frequencies

```
POST http://localhost:8080/api/admin/refresh-data
```

## Troubleshooting

-  If you encounter a "Port already in use" error, another application might be using port 8080. You can modify the port in the `WebServerLauncher.java` file.
-  If car data isn't loading, click the "Refresh Data" button to fetch the latest information from rental companies.
-  If the web crawler fails, ensure you have Firefox installed as it's required for the Selenium-based web crawling.
-  Check the console output for detailed error messages if you encounter any issues.

## Dependencies

The project uses the following main dependencies:

-  **org.json:json**: For JSON parsing and generation
-  **org.jsoup:jsoup**: For HTML parsing
-  **org.eclipse.jetty:jetty-server**: For the embedded web server
-  **javax.servlet:javax.servlet-api**: For servlet support
-  **org.seleniumhq.selenium:selenium-firefox-driver**: For web crawling

## Acknowledgments

-  This project was developed as part of a group assignment.
-  Thanks to all the team members who contributed to this project.
