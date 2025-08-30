package quiz;
import java.util.*;
public class quizproject {
	public static void main(String[] args) {
		 Scanner scanner = new Scanner(System.in);

	        Question q1 = new Question(
	            "What is the capital of France?",
	            new String[]{"Paris", "London", "Berlin", "Rome"},
	            "Paris", 10
	        );
	        Question q2 = new Question(
	            "Which planet is known as the Red Planet?",
	            new String[]{"Earth", "Mars", "Jupiter", "Venus"},
	            "Mars", 10
	        );
	        Question q3 = new Question(
	            "What is 2 + 2?",
	            new String[]{"3", "4", "5", "6"},
	            "4", 10
	        );
	        Question q4 = new Question(
	            "Who wrote 'Romeo and Juliet'?",
	            new String[]{"Shakespeare", "Dickens", "Hemingway", "Twain"},
	            "Shakespeare", 10
	        );
	        Question q5 = new Question(
	            "What is the boiling point of water (in °C)?",
	            new String[]{"90", "80", "100", "120"},
	            "100", 10
	        );

	        Question[] questions = {q1, q2, q3, q4, q5};
	        User user = new User("Ali", questions.length);
	        Quiz quiz = new Quiz("General Knowledge Quiz", questions, false, 0, user);

	        while (quiz.hasNextQuestion()) {
	            Question q = quiz.getNextQuestion();
	            System.out.println("\n" + q.questionText);
	            for (int i = 0; i < q.options.length; i++) {
	                System.out.println((i + 1) + ". " + q.options[i]);
	            }

	            int choice = 0;
	            while (choice < 1 || choice > q.options.length) {
	                System.out.print("Enter your choice (1-" + q.options.length + "): ");
	                if (scanner.hasNextInt()) {
	                    choice = scanner.nextInt();
	                } else {
	                    scanner.next();
	                }
	            }
	            q.userAnswer = q.options[choice - 1];
	            quiz.updateScore(q);
	        }

	        Results result = quiz.generateResults();
	        System.out.println("\nQuiz Completed!");
	        System.out.println("Correct Answers: " + result.correctAnswers);
	        System.out.println("Wrong Answers: " + result.wrongAnswers);
	        System.out.println("Percentage: " + result.percentage + "%");
	        System.out.println("Feedback: " + result.feedback);
	    }
	}

	class Question {
	    String questionText;
	    String[] options;
	    String correctAnswer;
	    String userAnswer;
	    int points;

	    public Question(String questionText, String[] options, String correctAnswer, int points) {
	        this.questionText = questionText;
	        this.options = options;
	        this.correctAnswer = correctAnswer;
	        this.points = points;
	    }

	    public boolean isCorrect() {
	        return userAnswer != null && userAnswer.equals(correctAnswer);
	    }
	}

	class User {
	    String name;
	    int currentScore;
	    Question[] answeredQuestions;
	    int answeredCount;

	    public User(String name, int maxQuestions) {
	        this.name = name;
	        this.currentScore = 0;
	        this.answeredQuestions = new Question[maxQuestions];
	        this.answeredCount = 0;
	    }

	    public void addAnsweredQuestion(Question q) {
	        if (answeredCount < answeredQuestions.length) {
	            answeredQuestions[answeredCount++] = q;
	        }
	    }
	}

	class Results {
	    int totalQuestions;
	    int correctAnswers;
	    int wrongAnswers;
	    double percentage;
	    String feedback;

	    public Results(Question[] questions, int total) {
	        this.totalQuestions = total;
	        this.correctAnswers = 0;

	        for (int i = 0; i < total; i++) {
	            if (questions[i] != null && questions[i].isCorrect()) {
	                correctAnswers++;
	            }
	        }

	        this.wrongAnswers = total - correctAnswers;
	        this.percentage = (correctAnswers / (double) total) * 100;
	        this.feedback = generateFeedback();
	    }

	    private String generateFeedback() {
	        if (percentage == 100) return "Excellent! Perfect score.";
	        if (percentage >= 75) return "Great job!";
	        if (percentage >= 50) return "Good effort.";
	        return "Needs improvement.";
	    }
	}

	class Quiz {
	    String title;
	    Question[] questions;
	    int totalQuestions;
	    int currentQuestionIndex;
	    User user;
	    boolean isTimed;
	    int timeLimit;
	    int score;

	    public Quiz(String title, Question[] questions, boolean isTimed, int timeLimit, User user) {
	        this.title = title;
	        this.questions = questions;
	        this.totalQuestions = questions.length;
	        this.currentQuestionIndex = 0;
	        this.user = user;
	        this.isTimed = isTimed;
	        this.timeLimit = timeLimit;
	        this.score = 0;
	    }

	    public boolean hasNextQuestion() {
	        return currentQuestionIndex < totalQuestions;
	    }

	    public Question getNextQuestion() {
	        return hasNextQuestion() ? questions[currentQuestionIndex++] : null;
	    }

	    public void updateScore(Question q) {
	        if (q.isCorrect()) {
	            score += q.points;
	            user.currentScore += q.points;
	        }
	        user.addAnsweredQuestion(q);
	    }

	    public Results generateResults() {
	        return new Results(user.answeredQuestions, totalQuestions);
	    

	}

}
