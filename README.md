import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class StockTradingPlatform {

    // Simulated market data
    private static final Map<String, Double> marketData = new HashMap<>();
    private static final Map<String, Integer> portfolio = new HashMap<>();
    
    static {
        // Initial market data
        marketData.put("AAPL", 150.0);
        marketData.put("GOOGL", 2800.0);
        marketData.put("AMZN", 3400.0);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String command;
        
        do {
            System.out.println("Commands: [buy, sell, view, quit]");
            System.out.print("Enter command: ");
            command = scanner.nextLine();
            
            switch (command.toLowerCase()) {
                case "buy":
                    buyStock(scanner);
                    break;
                case "sell":
                    sellStock(scanner);
                    break;
                case "view":
                    viewPortfolio();
                    break;
                case "quit":
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Unknown command. Please try again.");
            }
        } while (!command.equalsIgnoreCase("quit"));
        
        scanner.close();
    }

    private static void buyStock(Scanner scanner) {
        System.out.print("Enter stock symbol to buy: ");
        String symbol = scanner.nextLine().toUpperCase();
        
        if (!marketData.containsKey(symbol)) {
            System.out.println("Stock not found.");
            return;
        }
        
        System.out.print("Enter number of shares to buy: ");
        int shares = scanner.nextInt();
        scanner.nextLine();  // Consume newline
        
        double price = marketData.get(symbol);
        portfolio.put(symbol, portfolio.getOrDefault(symbol, 0) + shares);
        System.out.printf("Bought %d shares of %s at $%.2f each.%n", shares, symbol, price);
    }

    private static void sellStock(Scanner scanner) {
        System.out.print("Enter stock symbol to sell: ");
        String symbol = scanner.nextLine().toUpperCase();
        
        if (!portfolio.containsKey(symbol)) {
            System.out.println("You don't own any shares of this stock.");
            return;
        }
        
        System.out.print("Enter number of shares to sell: ");
        int shares = scanner.nextInt();
        scanner.nextLine();  // Consume newline
        
        if (shares > portfolio.get(symbol)) {
            System.out.println("You don't have enough shares to sell.");
            return;
        }
        
        double price = marketData.get(symbol);
        portfolio.put(symbol, portfolio.get(symbol) - shares);
        if (portfolio.get(symbol) == 0) {
            portfolio.remove(symbol);
        }
        System.out.printf("Sold %d shares of %s at $%.2f each.%n", shares, symbol, price);
    }

    private static void viewPortfolio() {
        if (portfolio.isEmpty()) {
            System.out.println("Your portfolio is empty.");
            return;
        }
        
        double totalValue = 0;
        System.out.println("Portfolio:");
        for (Map.Entry<String, Integer> entry : portfolio.entrySet()) {
            String symbol = entry.getKey();
            int shares = entry.getValue();
            double price = marketData.get(symbol);
            double value = shares * price;
            totalValue += value;
            System.out.printf("%s: %d shares at $%.2f each, worth $%.2f%n", symbol, shares, price, value);
        }
        System.out.printf("Total Portfolio Value: $%.2f%n", totalValue);
    }
}
