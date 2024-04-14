# Satellite Orbital Prediction

## Problem Statement

We want to explore alternative methods for predicting the orbital positions and potential collisions among satellites within the congested environment of Low Earth Orbit (LEO).

In addressing the complexities of Low Earth Orbit (LEO), we confront the uncertainties from current parameter estimations. The proliferation of space debris and the escalating satellite density have introduced considerable challenges in accurately forecasting orbital trajectories and potential collisions. We want to construct a predictive model to accommodate the inherent disorder and uncertainties prevalent in LEO.

As of November 2023, the number of satellites in LEO has reached a substantial count of 4059 according to data from [Space Debris Database](https://space.oscar.wmo.int/satellites). While some satellites possess the capability for refueling to maintain their orbits, a considerable portion are not designed for refueling and ultimately reach the end of their operational lifespan. Upon depletion of their fuel reserves, these satellites become retired and are either deliberately deorbited through controlled re-entry, burning up in the Earth's atmosphere, or directed into designated 'spacecraft cemeteries' [NASA's spacecraft graveyard](https://spaceplace.nasa.gov/spacecraft-graveyard/en/).

Contrarily, operational LEO satellites undergo periodic adjustments in their orbits, varying from minor corrections occurring between weekly to yearly intervals. This adjustment is essential for sustaining their orbital positions and avoiding potential collisions. For instance, the International Space Station (ISS) undergoes periodic altitude adjustments as evidenced by its fluctuating height over the past year, observable through data such as that provided by [Heavens Above](https://www.heavens-above.com/IssHeight.aspx).


## What is [TLE](https://en.wikipedia.org/wiki/Two-line_element_set)
TLE, or Two-Line Element set, is a standardized format used to encapsulate the position and velocity information of Earth-orbiting objects at a specific time. It serves as a snapshot providing crucial data about the trajectory and orbit of satellites or space debris, but it's only valid [within 5-10 days](https://service.eumetsat.int/tle/#:~:text=METOP%2C%20NOAA%2C%20SUOMI%20TLEs&text=Their%20validity%20arc%20spans%20from,similar%20accuracy%20for%20several%20weeks.).

This format is presented in text files, featuring two lines of 69-column ASCII characters, prefaced by a title line that identifies the object in focus. The first line of a TLE includes details like the satellite's identification number, epoch time, and coefficients representing orbital parameters. The second line contains additional orbital elements such as inclination, eccentricity, argument of perigee, mean anomaly, mean motion, and other pertinent data necessary for precise position calculations.


So for example, a TLE from International Space Station looks like this:
```
ISS (ZARYA)             
1 25544U 98067A   23346.67906868  .00008908  00000+0  16188-3 0  9999
2 25544  51.6402 164.4314 0001234  45.5486  50.9340 15.50349770429473
```

Here's the parameters that Two-Line Element set (TLE) used to describe satellite orbits:

1. **Satellite Identification Number:** A unique identifier assigned to a specific satellite for tracking and cataloging purposes.

2. **Epoch Time:** The moment at which the TLE was generated, indicating the time to which the orbital parameters refer.

3. **Orbital Parameters Coefficients:** These coefficients represent values used in mathematical formulas to define the satellite's orbital path, including adjustments for atmospheric drag and gravitational perturbations.

4. **Inclination:** The angle between the orbital plane of the satellite and the equatorial plane of the Earth, determining the tilt of the satellite's orbit.

5. **Eccentricity:** Describes the shape of the satellite's orbit, indicating how much it deviates from a perfect circle. Values range from 0 (circular) to 1 (highly elliptical).

6. **Argument of Perigee:** Defines the angle between the ascending node (where the satellite crosses the equatorial plane going north) and the point of closest approach to the Earth.

7. **Mean Anomaly:** Specifies the satellite's angular position along its elliptical orbit at a specific time relative to the point of perigee.

8. **Mean Motion:** Indicates the average angular velocity of the satellite along its orbit, measured in revolutions per day.

These TLE sets are widely utilized by satellite tracking systems, research institutions, and space agencies to forecast future satellite positions, aiding in collision avoidance strategies, mission planning, and orbital analyses in the increasingly crowded Low Earth Orbit environment.

## Why Bayesians might work?

The escalating number of objects in Low Earth Orbit (LEO) heightens the risk of collisions, exacerbating the creation of space debris and the potential for the [Kessler Syndrome](https://www.space.com/kessler-syndrome-space-debris#:~:text=The%20Kessler%20Syndrome%20is%20a,satellites%2C%20astronauts%20and%20mission%20planners.). This cycle poses severe threats to operational satellites and the viability of future space missions by rendering orbits unusable due to debris accumulation. Predicting space collisions necessitates trajectory calculations and evaluating collision probabilities. While traditional mathematical methods offer precision, they demand accurate position and velocity data for each object, often limited by uncertainties in tracking smaller debris.

Bayesian methods offers an alternative approach to precise mathematical calculations due to its ability to incorporate uncertainties and account for incomplete or imperfect data. Here are reasons why Bayesian methods could be advantageous in predicting collisions compared to traditional precise mathematical calculations:

1. **Accounting for Uncertainties:** In space, obtaining precise and complete data on all orbital parameters of objects is challenging due to various factors like tracking errors, irregular gravitational influences, and incomplete observational data. Bayesian methods excel in handling uncertainties inherent in such situations. They allow for the incorporation of prior knowledge or beliefs about the variables involved, and as new data becomes available, they continuously update the predictions through posterior probabilities. This approach helps in producing more realistic estimations of collision probabilities, even with imperfect or incomplete information.

2. **Adaptability and Continuous Learning:** Bayesian methods excel in versatile modeling, accommodating complex interactions among space objects while continuously learning from diverse data sources. This adaptability enables them to assess collision risks in dynamic Low Earth Orbit (LEO) environments by considering evolving factors like object positions and environmental changes.

3. **Decision-Making Under Uncertainty:** Space missions often require critical decision-making regarding collision avoidance maneuvers or satellite repositioning. Bayesian methods provide not only predictions but also probabilistic distributions, enabling decision-makers to assess the level of risk and make informed decisions under uncertainty.

In summary, exploring Bayesian methods for collision prediction in space offers a more robust and adaptable approach compared to precise mathematical calculations by effectively handling uncertainties, incorporating prior knowledge, adapting to new information, and enabling informed decision-making in a dynamically changing orbital environment.

## Assumptions and Limitations in this Project

1. Only using Geosynchronus satellite data from CELESTREK - https://celestrak.org/NORAD/elements/gp.php?GROUP=geo&FORMAT=tle

2. We aren't plotting all the traces for a satellite as generated by our MONTE CARLO model. Instead, we introduce uncertainity into it and then take the mean values for each parameter that helps determine orbital parameters

**The Intuition of the model**

Probabilistic approach to model the orbits of satellites. Traditional deterministic models would use single point estimates for parameters like mean motion, eccentricity, etc. However, this approach acknowledges uncertainties in these parameters and models them as probability distributions. This allows for a more robust understanding of the range of possible orbits for each satellite, considering uncertainties and variabilities in their orbital parameters.

The Monte Carlo Model runs the simulation on every single satellite as opposed to running a simualtion on all satellites.
