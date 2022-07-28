# First-portfolio-project
Analysis of public covid dataset



--To Select Data we are going to use

SELECT Location, Date, total_cases, new_cases, total_deaths, population
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Order By 1,2



--To find out the Percentage of total deaths over total cases.

SELECT Location, Date, total_cases, total_deaths, 
(total_deaths/total_cases)*100 AS Death_Percentage
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Order By 1,2



--add Where Clause

SELECT Location, Date, total_cases, total_deaths, 
(total_deaths/total_cases)*100 AS Death_Percentage
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Where Location = "India"
Order By 1,2
(Chances of dying from covid in your country=1.20%)



--To find out the percentage v of the total cases of population.

SELECT Location, Date, population, total_cases,
(total_cases/population)*100 AS Infected_Percentage
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Where Location ="India"
Order By 1,2
(Infected=3.10%)



--Countries that have the highest infection rates compared to population

SELECT Location, population, Max(total_cases) As Max_Infection_Count,
(Max(total_cases/population))*100 AS Infected_Percentage
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Group By Population, location
Order By Infected_percentage DESC


--Countries that have the highest death rates compared to population

SELECT Location, MAX(total_deaths) AS Max_deaths_Count
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Where continent is not null
Group By location 
Order By Max_deaths_Count DESC
(US has encountered the highest death count in covid. We got the world’s & continent’s count of death, for that we included IS NOT NULL for the continent column.)



--Continents with the highest death count

SELECT continent, MAX(total_deaths) AS Max_deaths_Count
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Where continent is not null
Group By continent 
Order By Max_deaths_Count DESC



--Global Death Percentage per day

SELECT date, sum(new_cases) AS Total_Cases, Sum(new_deaths) AS Total_Deaths, Sum(new_deaths)/sum(new_cases)*100 AS Death_Percentage
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Where continent is not null
Group By date
Order By 1,2



--Global Death Percentage

SELECT sum(new_cases) AS Total_Cases, Sum(new_deaths) AS Total_Deaths, Sum(new_deaths)/sum(new_cases)*100 AS Death_Percentage
FROM `my-portfolio-project-356910.Covid_Deaths.Deaths` 
Where continent is not null
Order By 1,2



--JOINS  - Looking for total population versus Vaccination

SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
FROM `my-portfolio-project-356910.Covid_Deaths.Vaccinations` AS vac
JOIN `my-portfolio-project-356910.Covid_Deaths.Deaths` AS dea
ON vac.location = dea.location
AND vac.date = dea.date
Where dea.continent is not null
order by 1,2,3



--Cumulation 

SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
sum(vac.new_vaccinations) OVER (partition By dea.location order by dea.location) AS cumulate_people_vaccinated
FROM `my-portfolio-project-356910.Covid_Deaths.Vaccinations` AS vac
JOIN `my-portfolio-project-356910.Covid_Deaths.Deaths` AS dea
ON vac.location = dea.location
AND vac.date = dea.date
Where dea.continent is not null
order by 3,2,1

