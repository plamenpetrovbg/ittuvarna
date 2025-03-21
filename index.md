---
title: Лабораторно упражнение 1
parent: Интернет технологии
layout: default
---

# Cъздаване на сървлети

Сървлетите са Java програми, които обслужват HTTP заявки и имплементират jakarta.servlet.Servlet интерфейса. Разработчиците на уеб приложения създават сървлети като наследяват jakarta.servlet.http.HttpServlet класа - абстрактен клас, който имплементира Servlet интерфейса и е специално проектиран за обслужване на HTTP заявки.

```
package Package;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

class CarDatabase {
    private static final List<Car> cars = Collections.synchronizedList(new ArrayList<>());`

    public static List<Car> getCars() {
        synchronized (cars) {
            return new ArrayList<>(cars);
        }
    }

    public static void addCar(Car car) {
        synchronized (cars) {
            cars.add(car);
        }
    }
}

@WebServlet("/cars/add")
public class CarRegistrationServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String regNumber = request.getParameter("regNumber");
        String brand = request.getParameter("brand");
        String model = request.getParameter("model");
        String owner = request.getParameter("owner");
        int year;

        try {
            year = Integer.parseInt(request.getParameter("year"));
        } catch (NumberFormatException e) {
            response.sendError(HttpServletResponse.SC_BAD_REQUEST, "Invalid year format");
            return;
        }

        CarDatabase.addCar(new Car(regNumber, brand, model, year, owner));

        response.setContentType("application/xml");
        PrintWriter out = response.getWriter();
        out.println("<?xml version=\"1.0\" encoding=\"UTF-8\"?>");
        out.println("<message>Car registered successfully</message>");
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.sendError(HttpServletResponse.SC_METHOD_NOT_ALLOWED, "Use POST to add a car.");
    }
}

class Car {
    private String regNumber, brand, model, owner;
    private int year;

    public Car() {}

    public Car(String regNumber, String brand, String model, int year, String owner) {
        this.regNumber = regNumber;
        this.brand = brand;
        this.model = model;
        this.year = year;
        this.owner = owner;
    }

    public String getRegNumber() { return regNumber; }
    public int getYear() { return year; }

    public String toXml() {
        return "<car><regNumber>" + regNumber + "</regNumber>" +
                "<brand>" + brand + "</brand>" +
                "<model>" + model + "</model>" +
                "<year>" + year + "</year>" +
                "<owner>" + owner + "</owner></car>";
    }
}
```

