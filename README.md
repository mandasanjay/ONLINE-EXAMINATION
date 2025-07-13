import java.util.*;
public class OnlineExamination {
    static Scanner sc = new Scanner(System.in);
    static String username = "sanjay";
    static String password = "2006";
    static boolean loggedIn = false;
    static Map<String, String> answers = new LinkedHashMap<>();
    static final int TIME_LIMIT_SECONDS = 30;
    static boolean timeUp = false;
    public static void main(String[] args) {
        login();
        if (!loggedIn) return;
        boolean exit = false;
        while (!exit) {
            System.out.println("\n=== MENU ===");
            System.out.println("1. Update Profile");
            System.out.println("2. Start Exam");
            System.out.println("3. Logout");
            System.out.print("Choose option: ");
            int choice = sc.nextInt();
            sc.nextLine();
            switch (choice) {
                case 1:
                    updateProfile();
                    break;
                case 2:
                    startExam();
                    break;
                case 3:
                    logout();
                    exit = true;
                    break;
                default:
                    System.out.println("Invalid option.");
            }
        }
    }
    static void login() {
        System.out.println("=== LOGIN ===");
        while (!loggedIn) {
            System.out.print("Username: ");
            String user = sc.nextLine();
            System.out.print("Password: ");
            String pass = sc.nextLine();
            if (user.equals(username) && pass.equals(password)) {
                loggedIn = true;
                System.out.println("✅ Login successful!");
            } else {
                System.out.println("❌ Invalid credentials. Try again.");
            }
        }
    }
    static void updateProfile() {
        System.out.println("\n--- Update Profile ---");
        System.out.println("1. Change Username");
        System.out.println("2. Change Password");
        System.out.print("Select option: ");
        int option = sc.nextInt();
        sc.nextLine();
        switch (option) {
            case 1:
                System.out.print("Enter new username: ");
                username = sc.nextLine();
                System.out.println("Username updated!");
                break;
            case 2:
                System.out.print("Enter old password: ");
                String oldPass = sc.nextLine();
                if (oldPass.equals(password)) {
                    System.out.print("Enter new password: ");
                    password = sc.nextLine();
                    System.out.println("Password updated!");
                } else {
                    System.out.println("Incorrect old password!");
                }
                break;
            default:
                System.out.println("Invalid choice.");
        }
    }
    static void startExam() {
        System.out.println("\n=== JAVA MCQ EXAM STARTED ===");
        System.out.println("Time limit: " + TIME_LIMIT_SECONDS + " seconds");
        Timer timer = new Timer();
        timeUp = false;
        timer.schedule(new TimerTask() {
            public void run() {
                timeUp = true;
                System.out.println("\n⏰ Time's up! Auto-submitting...");
                submitAnswers();
                logout();
                System.exit(0);
            }
        }, TIME_LIMIT_SECONDS * 1000);
        askQuestion("1. What is JVM in Java?\nA. Java Virtual Machine\nB. Java Variable Model\nC. Java Verified Mode\nD. None", "A");
        askQuestion("2. Which collection allows duplicates?\nA. Set\nB. Map\nC. List\nD. None", "C");
        askQuestion("3. What is method overloading?\nA. Multiple methods same name\nB. Multiple classes\nC. Multiple threads\nD. None", "A");
        askQuestion("4. What is the default value of boolean?\nA. true\nB. 1\nC. false\nD. null", "C");
        if (!timeUp) {
            timer.cancel();
            System.out.println("\n✅ You finished before time!");
            submitAnswers();
        }
    }
    static void askQuestion(String question, String correctAnswer) {
        if (timeUp) return;

        System.out.println("\n" + question);
        System.out.print("Your answer: ");
        String userAnswer = sc.nextLine().trim().toUpperCase();
        answers.put(question, userAnswer);
    }
    static void submitAnswers() {
        int score = 0;
        int total = answers.size();
        List<String> correct = List.of("A", "C", "A", "C");
        int i = 0;
        System.out.println("\n===== Exam Result =====");
        for (Map.Entry<String, String> entry : answers.entrySet()) {
            String q = entry.getKey();
            String ans = entry.getValue();
            String correctAns = correct.get(i);
            System.out.println("\nQ" + (i + 1) + ": " + q);
            System.out.println("Your Answer: " + ans);
            System.out.println("Correct Answer: " + correctAns);
            if (ans.equals(correctAns)) score++;
            i++;
        }
        System.out.println("\nScore: " + score + " / " + total);
        System.out.println("=========================");
    }
    static void logout() {
        System.out.println("\n🚪 Logging out...");
        loggedIn = false;
        System.out.println("👋 Thank you! Session closed.");
    }
}
