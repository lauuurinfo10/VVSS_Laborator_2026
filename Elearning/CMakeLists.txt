cmake_minimum_required(VERSION 3.20)
project(Elearning LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# 1. Indicăm calea Homebrew pentru Apple Silicon (unde se află unixODBC)
if(APPLE)
    list(APPEND CMAKE_PREFIX_PATH "/opt/homebrew/opt/unixodbc" "/opt/homebrew")
endif()

# 2. Căutăm biblioteca ODBC
find_package(ODBC REQUIRED)

# Adăugăm prefixul "Elearning/" și corectăm căile exacte conform structurii de pe disc
add_executable(Elearning
    Elearning/main.cpp
    Elearning/EntityLayer/User.cpp
    Elearning/EntityLayer/Student.cpp
    Elearning/IntegrationLayer/Logger/Logger.cpp
    Elearning/IntegrationLayer/Logger/LoggerSingleton.cpp
    Elearning/IntegrationLayer/Database/Database.cpp
    Elearning/IntegrationLayer/Environment/Environment.cpp
)

# 3. Adăugăm directoarele de include corectate
target_include_directories(Elearning PRIVATE
    Elearning
    Elearning/Utils
    Elearning/EntityLayer
    Elearning/IntegrationLayer/Logger
    Elearning/IntegrationLayer/Database
    Elearning/IntegrationLayer/Environment
)

# 4. Legăm biblioteca ODBC de executabil
target_link_libraries(Elearning PRIVATE
    ODBC::ODBC
)