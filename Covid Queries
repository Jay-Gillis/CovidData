-- Looking at total cases vs total deaths
SELECT location, population, total_cases,total_deaths
FROM `covid-deaths-367921.covid_deaths.covid_deaths`
order by 3,4;


-- shows local statistics in my country
SELECT total_deaths,total_cases,population
FROM `covid-deaths-367921.covid_deaths.covid_deaths`
WHERE location like '%states%';

-- looking at countries with highest infection rate
SELECT location,population,MAX(total_cases) as Highest_Infection,MAX((total_cases/population))*100 as Case_percentage
FROM `covid-deaths-367921.covid_deaths.covid_deaths`
group by location, population
order by Case_percentage desc;

-- Showing countries with highest death count per population

SELECT location, MAX(total_deaths) as Total_Death_Count
FROM `covid-deaths-367921.covid_deaths.covid_deaths`
WHERE continent is not null
group by location
order by Total_Death_Count desc;

--Let's break things down by continent

SELECT continent, MAX(total_deaths) as Total_Death_Count
FROM `covid-deaths-367921.covid_deaths.covid_deaths`
WHERE continent is not null
group by continent
order by Total_Death_Count desc;

-- Continent death count per population 


SELECT date,SUM(new_deaths) as total_deaths,SUM(new_cases)as total_cases, SUM(new_deaths)/SUM(new_cases)*100 as New_Death_Percentage
FROM `covid-deaths-367921.covid_deaths.covid_deaths`
WHERE continent is not null
group by date
order by 1,2;


-- Total Global

SELECT SUM(new_deaths) as total_deaths,SUM(new_cases)as total_cases, SUM(new_deaths)/SUM(new_cases)*100 as New_Death_Percentage
FROM `covid-deaths-367921.covid_deaths.covid_deaths`
WHERE continent is not null
order by 1,2 desc;


-- Now looking at vaccinations table

SELECT *
FROM `covid-deaths-367921.covid_vaccinations.covid_vaccinations` ;

-- Joining the Death table with Vaccination table

--I had to go into original raw data ( good thing I had it saved) to make sure I had two joinable columns in the two tables because the original work in Excel did NOT include deleting all columns to the left of vac table EXCEPT for date and location



SELECT dea.continent,dea.date,dea.location,dea.population,vac.new_vaccinations
FROM `covid-deaths-367921.covid_deaths.covid_deaths` dea
JOIN `covid-deaths-367921.covid_vaccinations.cov_vac_1` vac
  on dea.date = vac.date 
  and dea.location = vac.location
 Where dea.continent is not null and new_vaccinations is not null
  order by 2,3;

WITH PopVsVac 
AS
(
SELECT dea.continent,dea.date,dea.location,dea.population, vac.new_vaccinations,SUM(vac.new_vaccinations) OVER (partition by dea.location order by dea.location, dea.date) as Rolling_Vac
FROM `covid-deaths-367921.covid_deaths.covid_deaths` dea
JOIN `covid-deaths-367921.covid_vaccinations.cov_vac_1` vac
  on dea.date = vac.date 
  and dea.location = vac.location
Where dea.continent is not null and new_vaccinations is not null
-- order by dea.location;
)
SELECT *
FROM PopVsVac

