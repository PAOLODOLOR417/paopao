import java.util.*;

class Email {
    private String subject;
    private String body;
    private String sender;
    private String category;

    public Email(String subject, String body, String sender) {
        this.subject = subject;
        this.body = body;
        this.sender = sender;
        this.category = "Uncategorized"; // Default category
    }

    public String getSubject() {
        return subject;
    }

    public String getBody() {
        return body;
    }

    public String getSender() {
        return sender;
    }

    public String getCategory() {
        return category;
    }

    public void setCategory(String category) {
        this.category = category;
    }

    public String toString() {
        return "Subject: " + subject + "\nSender: " + sender + "\nCategory: " + category + "\n";
    }
}

class EmailCategorizer {
    private static final List<String> DEAL_KEYWORDS = Arrays.asList("deal", "discount", "offer", "sale");
    private static final List<String> JOB_KEYWORDS = Arrays.asList("job", "position", "career", "hiring");
    private static final List<String> NEWSLETTER_KEYWORDS = Arrays.asList("newsletter", "update", "subscription");
    private static final List<String> SPAM_KEYWORDS = Arrays.asList("win", "free", "prize", "lottery", "click now");

    public static String categorizeEmail(Email email) {
        String subject = email.getSubject().toLowerCase();
        String body = email.getBody().toLowerCase();

        // Check for Spam first
        if (containsSpamKeywords(subject) || containsSpamKeywords(body)) {
            return "Spam";
        }

        // Check for Deals
        if (containsKeywords(subject, DEAL_KEYWORDS) || containsKeywords(body, DEAL_KEYWORDS)) {
            return "Ads";
        }

        // Check for Jobs
        if (containsKeywords(subject, JOB_KEYWORDS) || containsKeywords(body, JOB_KEYWORDS)) {
            return "Job Offers";
        }

        // Check for Newsletters
        if (containsKeywords(subject, NEWSLETTER_KEYWORDS) || containsKeywords(body, NEWSLETTER_KEYWORDS)) {
            return "News";
        }

        return "Others";
    }

    private static boolean containsKeywords(String text, List<String> keywords) {
        for (String keyword : keywords) {
            if (text.contains(keyword)) {
                return true;
            }
        }
        return false;
    }

    private static boolean containsSpamKeywords(String text) {
        return containsKeywords(text, SPAM_KEYWORDS);
    }
}

class User {
    private String username;
    private List<Email> emails;
    private List<Email> newEmails; // List for newly added emails

    public User(String username) {
        this.username = username;
        this.emails = new ArrayList<>();
        this.newEmails = new ArrayList<>();
    }

    public String getUsername() {
        return username;
    }

    public List<Email> getEmails() {
        return emails;
    }

    public void addEmail(Email email) {
        newEmails.add(email);
        emails.add(email); // Adds email to both lists
    }

    public List<Email> getNewEmails() {
        return newEmails;
    }

    public void deleteAllEmails() {
        emails.clear();
        newEmails.clear();
    }

    public void categorizeEmails() {
        // Categorize the emails in the list
        for (Email email : emails) {
            String category = EmailCategorizer.categorizeEmail(email);
            email.setCategory(category);
        }
    }

    public List<Email> getEmailsByCategory(String category) {
        List<Email> categorizedEmails = new ArrayList<>();
        for (Email email : emails) {
            if (email.getCategory().equalsIgnoreCase(category)) {
                categorizedEmails.add(email);
            }
        }
        return categorizedEmails;
    }
}

public class Email_Categorization_System {
    private static final Map<String, User> users = new HashMap<>();

    public static void main(String[] args) {
        // Creating users and adding emails to their accounts
        User user1 = new User("sampleACC1");
        user1.addEmail(new Email("50% Off on Latest Electronics", "Don't miss out on the best deal of the season! Get your favorite gadgets at a 50% discount.", "deals@shop.com"));
        user1.addEmail(new Email("Job Opening: Software Engineer", "We are hiring! Apply for the Software Engineer position today.", "jobs@company.com"));
        user1.addEmail(new Email("Weekly Newsletter - November Edition", "This week's edition of our newsletter is out. Check the latest updates.", "newsletter@company.com"));
        user1.addEmail(new Email("Congratulations! You've won a free iPhone!", "You’ve won a prize! Claim your free iPhone now!", "spammer@fake.com"));
        user1.addEmail(new Email("Seasonal Sale on Gadgets", "Exclusive offer! Get 40% off on all electronic gadgets.", "ads@shop.com"));
        user1.addEmail(new Email("Marketing Internship Openings", "Looking for marketing interns. Apply now.", "hr@company.com"));

        users.put(user1.getUsername(), user1);

        // User login system
        Scanner scanner = new Scanner(System.in);
        System.out.println("Enter your username to log in:");
        String username = scanner.nextLine();

        if (users.containsKey(username)) {
            System.out.println("Login successful!");

            // Categorize emails for the logged-in user
            User loggedInUser = users.get(username);
            List<Email> userEmails = loggedInUser.getEmails();

            // Categorize the emails into proper categories
            loggedInUser.categorizeEmails();

            // Main menu
            int choice;
            while (true) {
                System.out.println("\nChoose an option:");
                System.out.println("1. View Emails by Category");
                System.out.println("2. View All Emails");
                System.out.println("3. Add a New Email");
                System.out.println("4. Delete all emails");
                System.out.println("5. Exit");
                System.out.print("Enter your choice: ");

                choice = scanner.nextInt();
                scanner.nextLine();  // Clear the buffer
                if (choice == 1) {
                    // Option 1: View emails by category
                    viewEmailsByCategory(loggedInUser, scanner);
                } else if (choice == 2) {
                    // Option 2: View all emails
                    viewAllEmails(loggedInUser, scanner);
                } else if (choice == 3) {
                    // Option 3: Add a new email
                    addNewEmail(loggedInUser, scanner);
                } else if (choice == 4) {
                    // Option 4: Delete all emails
                    loggedInUser.deleteAllEmails();
                    System.out.println("All emails deleted.");
                } else if (choice == 5) {
                    // Option 5: Exit
                    System.out.println("Goodbye!");
                    break;
                } else {
                    System.out.println("Invalid choice. Please try again.");
                }
            }
        }
    }

    private static void viewEmailsByCategory(User loggedInUser, Scanner scanner) {
        System.out.println("\nChoose the category you want to view:");
        System.out.println("1. Spam");
        System.out.println("2. Ads");
        System.out.println("3. Job Offers");
        System.out.println("4. News");
        System.out.println("5. Others");
        System.out.print("Enter your choice: ");
        int categoryChoice = scanner.nextInt();
        scanner.nextLine();  // Clear the buffer

        List<Email> categorizedEmails = new ArrayList<>();
        switch (categoryChoice) {
            case 1: categorizedEmails = loggedInUser.getEmailsByCategory("Spam"); break;
            case 2: categorizedEmails = loggedInUser.getEmailsByCategory("Ads"); break;
            case 3: categorizedEmails = loggedInUser.getEmailsByCategory("Job Offers"); break;
            case 4: categorizedEmails = loggedInUser.getEmailsByCategory("News"); break;
            case 5: categorizedEmails = loggedInUser.getEmailsByCategory("Others"); break;
            default: System.out.println("Invalid choice."); return;
        }

        if (categorizedEmails.isEmpty()) {
            System.out.println("No emails found in this category.");
        } else {
            System.out.println("\nDisplaying Emails:");
            for (Email email : categorizedEmails) {
                System.out.println(email);
            }
        }

        // Back to the main menu
        System.out.println("\nType 'back' to return to the main menu.");
        String input = scanner.nextLine();
        while (!input.equalsIgnoreCase("back")) {
            System.out.println("Invalid input. Please type 'back' to return to the main menu.");
            input = scanner.nextLine();
        }
    }

    private static void viewAllEmails(User loggedInUser, Scanner scanner) {
        List<Email> allEmails = loggedInUser.getEmails();
        if (allEmails.isEmpty()) {
            System.out.println("No emails available.");
        } else {
            System.out.println("\nDisplaying All Emails:");
            for (Email email : allEmails) {
                System.out.println(email);
            }
        }

        // Prompt the user to type "back" to return to the main menu
        System.out.println("\nType 'back' to return to the main menu.");
        String input = scanner.nextLine();
        while (!input.equalsIgnoreCase("back")) {
            System.out.println("Invalid input. Please type 'back' to return to the main menu.");
            input = scanner.nextLine();
        }
    }

    private static void addNewEmail(User loggedInUser, Scanner scanner) {
        System.out.println("\nEnter the subject of the email:");
        String subject = scanner.nextLine();

        System.out.println("Enter the body of the email:");
        String body = scanner.nextLine();

        System.out.println("Enter the sender of the email:");
        String sender = scanner.nextLine();

        // Create the new email
        Email newEmail = new Email(subject, body, sender);

        // Categorize the new email
        String category = EmailCategorizer.categorizeEmail(newEmail);
        newEmail.setCategory(category);

        // Add to the user's new emails list
        loggedInUser.addEmail(newEmail);

        System.out.println("\nNew email added successfully.");
        System.out.println("\nNew Emails:");
        System.out.println(newEmail);

        // Back to the main menu
        System.out.println("\nType 'back' to return to the main menu.");
        String input = scanner.nextLine();
        while (!input.equalsIgnoreCase("back")) {
            System.out.println("Invalid input. Please type 'back' to return to the main menu.");
            input = scanner.nextLine();
        }
    }
}
