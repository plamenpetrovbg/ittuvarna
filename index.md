---
title: Home
layout: home
---

This is a *bare-minimum* template to create a Jekyll site that uses the [Just the Docs] theme.

## Car Registration Servlet

Below is an example of a simple servlet that allows registering cars in a synchronized database. The servlet handles `POST` requests to add cars and responds with XML messages.

### Code Example
`
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
    private static final List<Car> cars = Collections.synchronizedList(new ArrayList<>());

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
`
For more details on using servlets and handling requests, refer to the [Jakarta Servlet Documentation](https://jakarta.ee/specifications/servlet/).

[Just the Docs]: https://just-the-docs.github.io/just-the-docs/

