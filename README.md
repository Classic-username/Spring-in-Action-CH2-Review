# Spring-in-Action-CH2-Review

CH2 summary according to Chat GPT

Chapter 2 of "Spring in Action, Sixth Edition" focuses on developing web applications using Spring MVC. The key topics covered in this chapter include:

Presenting Model Data in the Browser: This section discusses how to display data from your application in a web browser. It involves creating controllers and defining views to present the data.

Processing and Validating Form Input: This part explains how to handle user input from forms, including validating the data before processing it. It covers declaring validation rules, performing validation during form binding, and displaying validation errors to users.

Choosing a View Template Library: The chapter explores various view template libraries that can be used with Spring MVC, such as Thymeleaf, FreeMarker, and others. It also touches on caching templates for performance optimization.

Working with View Controllers: This involves setting up simple controllers that handle requests for specific views without requiring a lot of logic.

The chapter also includes practical examples by building functionality for the "Taco Cloud" application, such as enabling users to design custom tacos and process their orders. It provides a hands-on approach to learning by implementing these features step-by-step​ (liveBook · Manning)​​ (O'Reilly Media)​​ (liveBook · Manning)​.

To practice this chapter, set up a Spring Boot project, create controllers and views, and implement form handling with validation. Experiment with different view template libraries and practice setting up simple view controllers to solidify your understanding.



o practice the concepts from Chapter 2 of "Spring in Action, Sixth Edition," you can create a simple web application called "Bookstore Manager." This project will help you get hands-on experience with presenting model data, processing form inputs, validating data, and choosing a view template library. Here’s a step-by-step guide for the project:

1. Set Up the Project
Initialize a Spring Boot Project:

Use Spring Initializr to create a new Spring Boot project with dependencies such as Spring Web, Thymeleaf (or any other template engine), and Spring Boot DevTools.
Import the Project:

Import the project into your preferred IDE (IntelliJ IDEA, Eclipse, VS Code).
2. Define the Domain Model
Create a simple domain model for the Bookstore:

package com.example.bookstore;

```
public class Book {
    private String title;
    private String author;
    private double price;

    // Getters and Setters
}
```

3. Create a Controller
Create a controller to handle requests and display data:

```
package com.example.bookstore;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

import java.util.ArrayList;
import java.util.List;

@Controller
public class BookController {

    private List<Book> books = new ArrayList<>();

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("books", books);
        return "home";
    }

    @GetMapping("/add")
    public String addBookForm(Model model) {
        model.addAttribute("book", new Book());
        return "addBook";
    }

    @PostMapping("/add")
    public String addBookSubmit(@ModelAttribute Book book, Model model) {
        books.add(book);
        return "redirect:/";
    }
}
```

4. Create Views with Thymeleaf
home.html
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Bookstore</title>
</head>
<body>
<h1>Bookstore</h1>
<table>
    <tr>
        <th>Title</th>
        <th>Author</th>
        <th>Price</th>
    </tr>
    <tr th:each="book : ${books}">
        <td th:text="${book.title}">Title</td>
        <td th:text="${book.author}">Author</td>
        <td th:text="${book.price}">Price</td>
    </tr>
</table>
<a href="/add">Add a new book</a>
</body>
</html>
```

addBook.html

```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <title>Add Book</title>
</head>
<body>
<h1>Add a new Book</h1>
<form action="#" th:action="@{/add}" th:object="${book}" method="post">
    <div>
        <label for="title">Title:</label>
        <input type="text" id="title" th:field="*{title}" />
    </div>
    <div>
        <label for="author">Author:</label>
        <input type="text" id="author" th:field="*{author}" />
    </div>
    <div>
        <label for="price">Price:</label>
        <input type="number" step="0.01" id="price" th:field="*{price}" />
    </div>
    <div>
        <button type="submit">Add Book</button>
    </div>
</form>
</body>
</html>
```

5. Add Validation
Use annotations to validate form input:

```
package com.example.bookstore;

import javax.validation.constraints.Min;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;

public class Book {
    @NotNull
    @Size(min = 1, message = "Title is required")
    private String title;

    @NotNull
    @Size(min = 1, message = "Author is required")
    private String author;

    @Min(value = 0, message = "Price must be positive")
    private double price;

    // Getters and Setters
}
```

Update the controller to handle validation errors:

```
package com.example.bookstore;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

import javax.validation.Valid;
import java.util.ArrayList;
import java.util.List;

@Controller
public class BookController {

    private List<Book> books = new ArrayList<>();

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("books", books);
        return "home";
    }

    @GetMapping("/add")
    public String addBookForm(Model model) {
        model.addAttribute("book", new Book());
        return "addBook";
    }

    @PostMapping("/add")
    public String addBookSubmit(@Valid @ModelAttribute Book book, BindingResult result, Model model) {
        if (result.hasErrors()) {
            return "addBook";
        }
        books.add(book);
        return "redirect:/";
    }
}
```

6. Test the Application
Run your Spring Boot application and test the following:

Access the home page and verify the list of books is displayed.
Add a new book using the form and check that it appears in the list.
Try submitting invalid data to see if validation messages are displayed.

7. Experiment with Additional Features
To further enhance your project, you can:

Add more fields to the Book class (e.g., publication date, genre).
Implement delete and update functionalities for books.
Explore other view template libraries like FreeMarker or Mustache.
This project will give you practical experience with the core concepts covered in Chapter 2 of "Spring in Action, Sixth Edition."