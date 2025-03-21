---
title: Лабораторно упражнение 3
parent: Интернет технологии
layout: default
---

## Java Architecture for XML Binding

JAXB осигурява бърз и удобен начин за обвързване на XML схеми и Java представителства, като улеснява разработчиците на Java да включват XML данни и функции за обработка в Java приложения. Като част от този процес, JAXB предоставя методи за демаркиране (четене) на документи от XML екземпляри в дървета със съдържание на Java и след това преобразуване (писане) на дървета със съдържание на Java обратно в документи на XML инстанции. JAXB предоставя също начин за генериране на XML схема от Java обекти.

```
package Package;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.List;
import java.util.Comparator;

@WebServlet("/cars")
public class ShowAllCarsServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("application/xml");
        PrintWriter out = response.getWriter();

        List<Car> sortedCars = CarDatabase.getCars();
        sortedCars.sort(Comparator.comparingInt(Car::getYear));

        out.println("<?xml version=\"1.0\" encoding=\"UTF-8\"?>");
        out.println("<cars>");
        for (Car car : sortedCars) {
            out.println(car.toXml());
        }
        out.println("</cars>");
    }
}
```

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Car Registration</title>
</head>
<body>
<h2>Register a Car</h2>
<form action="http://localhost:8081/MavenUniProject_war_exploded/cars/add" method="POST">
    <label>Reg Number: <input type="text" name="regNumber" required></label><br>
    <label>Brand: <input type="text" name="brand" required></label><br>
    <label>Model: <input type="text" name="model" required></label><br>
    <label>Year: <input type="number" name="year" required></label><br>
    <label>Owner: <input type="text" name="owner" required></label><br>
    <button type="submit">Register</button>
</form>
</body>
</html>
```

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Search Car</title>
  <script>
    function searchCar(event) {
      event.preventDefault(); // Prevent default form submission

      let regNumber = document.getElementById("regNumber").value;
      let url = `http://localhost:8081/MavenUniProject_war_exploded/cars/view?regNumber=${encodeURIComponent(regNumber)}`;

      fetch(url)
              .then(response => response.text()) // Get response as text
              .then(data => {
                let responseContainer = document.getElementById("response");
                responseContainer.textContent = data; // Display raw XML response
              })
              .catch(error => console.error("Error:", error));
    }
  </script>
</head>
<body>
<h2>Search for a Car</h2>
<form id="searchForm" onsubmit="searchCar(event)">
  <label>Reg Number: <input type="text" id="regNumber" name="regNumber" required></label>
  <button type="submit">Search</button>
</form>

<h3>Search Result:</h3>
<pre id="response"></pre>
</body>
</html>
```
