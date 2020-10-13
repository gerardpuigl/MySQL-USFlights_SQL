-- Quantitat de registres de la taula de vols --

SELECT COUNT(flightID) AS total FROM flights;

-- Retard promig de sortida i arribada segons l’aeroport origen --

-- Observació: Algunes dades difereixen respecte als resultats exemple de l'exercici, per exemple ALB --

SELECT Origin,AVG(ArrDelay) AS prom_arribades,AVG(DepDelay) AS prom_sortides FROM flights GROUP BY Origin ORDER BY Origin ASC; 

-- Retard promig d’arribada dels vols, per mesos, anys i segons l’aeroport origen. --
-- A més, volen que els resultat es mostrin de la següent forma (fixa’t en l’ordre de les files) --

SELECT Origin,colYear,colMonth,AVG(ArrDelay) AS prom_arribades FROM flights GROUP BY Origin,colYear,colMonth ORDER BY Origin ASC; 

-- Retard promig d’arribada dels vols, per mesos, anys i segons l’aeroport origen (mateixa consulta que abans i amb el mateix ordre). 
-- Però a més, ara volen que en comptes del codi de l’aeroport es mostri el nom de la ciutat.

-- Observació: Algunes dades difereixen respecte als resultats exemple de l'exercici, per exemple no hi ha cap vol a Albany el 1989--

SELECT City,colYear,colMonth,AVG(ArrDelay) AS prom_arribades FROM flights
INNER JOIN usairports ON flights.origin=usairports.IATA
GROUP BY Origin,colYear,colMonth ORDER BY City,colYear,colMonth ASC;

-- Les companyies amb més vols cancelats, per mesos i any. --
-- A més, han d’estar ordenades de forma que les companyies amb més cancel·lacions apareguin les primeres --

SELECT UniqueCarrier,colYear,colMonth,SUM(Cancelled) AS total_cancelled FROM flights 
GROUP BY UniqueCarrier,colYear,colMonth 
ORDER BY SUM(Cancelled) DESC,colYear,colMonth ASC;

-- L’identificador dels 10 avions que més distància han recorregut fent vols.--

SELECT TailNum,SUM(Distance) AS totalDistance FROM flights WHERE TailNum != ''
GROUP BY TailNum ORDER BY SUM(Distance) DESC LIMIT 10;

-- Companyies amb el seu retard promig només d’aquelles les quals els seus vols 
-- arriben al seu destí amb un retràs promig major de 10 minut

SELECT UniqueCarrier,AVG(ArrDelay) AS avgDelay FROM flights 
GROUP BY UniqueCarrier 
HAVING  avgDelay>10
ORDER BY avgDelay DESC;
