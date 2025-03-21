---
title: Лабораторно упражнение 2
parent: Интернет технологии
layout: default
---

## Extensible Markup Language
XML документ се състои от елементи, като всеки елемент има начален етикет, съдържание и етикет за край. XML документа трябва да има точно един главен елемент (т.е. един етикет, който обгражда останалите тагове). XML прави разлика между малки и главни букви.

XML файлът трябва да е добре оформен. Това означава, че той трябва да се отговаря на следните условия:

XML документ винаги започва с prolog, който описва XML файла.
XML документ винаги започва с който описва XML файла. Този пролог може да бъде минимален, например:

```
package Package;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

@WebServlet("/cars/view")
public class ShowCarServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String regNumber = request.getParameter("regNumber");
        response.setContentType("application/xml");
        PrintWriter out = response.getWriter();

        for (Car car : CarDatabase.getCars()) {
            if (car.getRegNumber().equals(regNumber)) {
                out.println("<?xml version=\"1.0\" encoding=\"UTF-8\"?>");
                out.println(car.toXml());
                return;
            }
        }
        out.println("<?xml version=\"1.0\" encoding=\"UTF-8\"?>");
        out.println("<message>Car not found</message>");
    }
}
```
