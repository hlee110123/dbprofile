# install dbprofile package from GITHUB 
remotes::install_github("hlee110123/dbprofile")

# install packages 
install.packages("dbprofile")
install.packages("DBI")
install.packages("DatabaseConnector")
install.packages("rJava")

# Load packages
library(rJava)
library(DBI)
library(DatabaseConnector)
library(dbprofile)

# Verify change of Java setting in R Environment
Sys.getenv('JAVA_OPTIONS')

# Set your JAVA_HOME environment variable (set to path where Java was installed)
Sys.setenv(DATABASECONNECTOR_JAR_FOLDER = "#your JAR FOLDER directory") 

# Setup the connection details for your OMOP instance
connectionDetails <- createConnectionDetails(   
  dbms = #your dbms,   
  server = #your server,   
  user = #your username,   
  password = #your password,   
  port = #your port number,   
  pathToDriver = Sys.getenv('DATABASECONNECTOR_JAR_FOLDER')) ## Establish a connection using the DatabaseConnector "connect" function 

# Connect to the database
conn <- connect(connectionDetails)

# Get counts for a specific category
respiratory_count <- get_category_count(conn, "dbo", "respiratory")
print(paste("Respiratory patients (2016-2024):", respiratory_count))

# Get all prevalence rates
prevalence_rates <- get_prevalence_rates(conn, "dbo")

# View results sorted by prevalence
sorted_results <- prevalence_rates[order(-prevalence_rates$prevalence_rate), ]
print(sorted_results)

# Export to CSV
write.csv(prevalence_rates, "disease_prevalence_2016_2024.csv", row.names = FALSE)

# After exporting to CSV, you can sumit the file ("disease_prevalence_2016_2024.csv") vis email attachment ("hlee292@jh.edu")
