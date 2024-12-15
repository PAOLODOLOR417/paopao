Email Categorization System
Overview
The Email Categorization System is a Java-based application designed to help users organize and manage their emails efficiently. It categorizes emails into predefined categories such as Spam, Ads, Job Offers, News, and Others, based on their content. The system provides an interactive menu for users to:

View emails by category.
Add new emails (which are automatically categorized).
Delete all emails.
Exit the application.
Features
Automated Categorization: Emails are categorized based on keywords in the subject and body.
User-Friendly Interface: Simple menu-driven interface for easy interaction.
Scalable Design: Built with Object-Oriented Programming (OOP) principles for future enhancements.
Key OOP Principles Used
1. Abstraction
The Email class encapsulates email details, exposing only necessary methods for interaction (e.g., getSubject()).
2. Encapsulation
Email properties like subject, body, and sender are protected and accessed only via getter and setter methods.
3. Polymorphism
The system design allows for future extensions, where specialized email classes can override common methods like toString().
How It Works

Login:
Users log in using a predefined username (e.g., "sampleACC1").
Menu Options:
View Emails by Category: Lists emails in a specific category.
View All Emails: Displays all emails with their categories.
Add a New Email: Prompts the user to input email details, which are then automatically categorized.
Delete All Emails: Clears the email list.
Exit: Exits the program.
Categorization Logic:
Uses keyword matching to categorize emails into predefined groups.
Getting Started
Requirements:
Java Development Kit (JDK) installed.
Java IDE (e.g., IntelliJ IDEA, Eclipse) or a terminal for execution.
Running the Program:
Compile the program using javac Email_Categorization_System.java.
Run the program using java Email_Categorization_System.
Future Enhancements



