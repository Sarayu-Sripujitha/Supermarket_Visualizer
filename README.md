import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.Random;

public class SupermarketVisualizer {

    // Products, stock status, and prices
    static String[] products = {
            "🍎 Apple", "🥦 Broccoli", "🥕 Carrot", "🍪 Cookies", "🥤 Juice",
            "🍞 Bread", "🧀 Cheese", "🍫 Chocolate", "🍇 Grapes", "🥔 Potato",
            "🥛 Milk", "🧈 Butter", "🍚 Rice", "🍅 Tomato", "🧅 Onion"
    };

    static boolean[] inStock = {
            true, false, true, true, false,
            true, true, false, true, true,
            true, false, true, true, false
    };

    static int[] prices = {
            50, 30, 20, 40, 25,
            35, 60, 45, 55, 15,
            42, 70, 90, 33, 22
    };

    static JPanel panel;

    public static void main(String[] args) {
        JFrame frame = new JFrame("🍎🥦 Supermarket Visualizer 🥤🍪");
        frame.setSize(800, 600);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());

        // Main product display panel
        panel = new JPanel();
        panel.setLayout(new GridLayout(0, 3, 15, 15)); // 3 columns like shelves
        panel.setBackground(new Color(245, 245, 245));
        updateDisplay();

        // Buttons
        JButton sortButton = new JButton("📊 Sort by Price");
        JButton shuffleButton = new JButton("🔀 Shuffle");

        styleButton(sortButton);
        styleButton(shuffleButton);

        // Sort button action
        sortButton.addActionListener(e -> {
            bubbleSort();
            updateDisplay();
        });

        // Shuffle button action
        shuffleButton.addActionListener(e -> {
            shuffle();
            updateDisplay();
        });

        // Button panel
        JPanel buttonPanel = new JPanel();
        buttonPanel.setBackground(Color.WHITE);
        buttonPanel.add(sortButton);
        buttonPanel.add(shuffleButton);

        // Title
        JLabel title = new JLabel(" 🛒 Welcome to the Supermarket 🛒 ", JLabel.CENTER);
        title.setFont(new Font("SansSerif", Font.BOLD, 26));
        title.setOpaque(true);
        title.setBackground(new Color(76, 175, 80));
        title.setForeground(Color.WHITE);
        title.setBorder(BorderFactory.createEmptyBorder(15, 10, 15, 10));

        frame.add(title, BorderLayout.NORTH);
        frame.add(new JScrollPane(panel), BorderLayout.CENTER);
        frame.add(buttonPanel, BorderLayout.SOUTH);

        frame.setVisible(true);
    }

    // Update product display with styled "cards"
    static void updateDisplay() {
        panel.removeAll();
        for (int i = 0; i < products.length; i++) {
            JPanel card = new JPanel();
            card.setLayout(new BorderLayout());
            card.setBorder(BorderFactory.createCompoundBorder(
                    BorderFactory.createLineBorder(new Color(200, 200, 200), 1, true),
                    BorderFactory.createEmptyBorder(10, 10, 10, 10)
            ));
            card.setBackground(new Color(255, 250, 230)); // soft yellow

            JLabel nameLabel = new JLabel(products[i], JLabel.CENTER);
            nameLabel.setFont(new Font("SansSerif", Font.BOLD, 18));

            JLabel priceLabel = new JLabel("₹" + prices[i], JLabel.CENTER);
            priceLabel.setFont(new Font("SansSerif", Font.PLAIN, 16));
            priceLabel.setForeground(new Color(33, 150, 243));

            JLabel stockLabel = new JLabel(inStock[i] ? "✅ In Stock" : "❌ Out of Stock", JLabel.CENTER);
            stockLabel.setFont(new Font("SansSerif", Font.PLAIN, 14));
            stockLabel.setForeground(inStock[i] ? new Color(0, 128, 0) : Color.RED);

            card.add(nameLabel, BorderLayout.NORTH);
            card.add(priceLabel, BorderLayout.CENTER);
            card.add(stockLabel, BorderLayout.SOUTH);

            panel.add(card);
        }
        panel.revalidate();
        panel.repaint();
    }

    // Bubble Sort by price
    static void bubbleSort() {
        int n = prices.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = 0; j < n - i - 1; j++) {
                if (prices[j] > prices[j + 1]) {
                    swap(j, j + 1);
                }
            }
        }
    }

    // Shuffle products randomly
    static void shuffle() {
        Random rand = new Random();
        for (int i = products.length - 1; i > 0; i--) {
            int j = rand.nextInt(i + 1);
            swap(i, j);
        }
    }

    // Swap elements in all arrays
    static void swap(int i, int j) {
        int tempPrice = prices[i];
        prices[i] = prices[j];
        prices[j] = tempPrice;

        String tempProduct = products[i];
        products[i] = products[j];
        products[j] = tempProduct;

        boolean tempStock = inStock[i];
        inStock[i] = inStock[j];
        inStock[j] = tempStock;
    }

    // Style buttons
    static void styleButton(JButton button) {
        button.setFont(new Font("SansSerif", Font.BOLD, 16));
        button.setBackground(new Color(76, 175, 80));
        button.setForeground(Color.WHITE);
        button.setFocusPainted(false);
        button.setBorder(BorderFactory.createEmptyBorder(10, 20, 10, 20));
    }
}
