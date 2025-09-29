import javax.swing.*;
import java.awt.*;
import java.util.Random;

// Node for Doubly Linked List
class ProductNode {
    String name;
    boolean inStock;
    int price;
    ProductNode prev, next;

    ProductNode(String name, boolean inStock, int price) {
        this.name = name;
        this.inStock = inStock;
        this.price = price;
    }
}

public class SupermarketVisualizerDLL {

    static ProductNode head = null; // doubly linked list head
    static JPanel panel;

    public static void main(String[] args) {
        // Create product linked list
        addProduct("🍎 Apple", true, 50);
        addProduct("🥦 Broccoli", false, 30);
        addProduct("🥕 Carrot", true, 20);
        addProduct("🍪 Cookies", true, 40);
        addProduct("🥤 Juice", false, 25);
        addProduct("🍞 Bread", true, 35);
        addProduct("🧀 Cheese", true, 60);
        addProduct("🍫 Chocolate", false, 45);
        addProduct("🍇 Grapes", true, 55);
        addProduct("🥔 Potato", true, 15);
        addProduct("🥛 Milk", true, 42);
        addProduct("🧈 Butter", false, 70);
        addProduct("🍚 Rice", true, 90);
        addProduct("🍅 Tomato", true, 33);
        addProduct("🧅 Onion", false, 22);

        // Create JFrame
        JFrame frame = new JFrame("🍎🥦 Supermarket Visualizer (Linked List) 🥤🍪");
        frame.setSize(800, 600);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());

        // Main product display panel
        panel = new JPanel();
        panel.setLayout(new GridLayout(0, 3, 15, 15));
        panel.setBackground(new Color(245, 245, 245));
        updateDisplay();

        // Buttons
        JButton sortButton = new JButton("📊 Sort by Price");
        JButton shuffleButton = new JButton("🔀 Shuffle");

        styleButton(sortButton);
        styleButton(shuffleButton);

        // Button actions
        sortButton.addActionListener(e -> {
            bubbleSort();
            updateDisplay();
        });

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

    // Add product to doubly linked list
    static void addProduct(String name, boolean inStock, int price) {
        ProductNode newNode = new ProductNode(name, inStock, price);
        if (head == null) {
            head = newNode;
        } else {
            ProductNode temp = head;
            while (temp.next != null) temp = temp.next;
            temp.next = newNode;
            newNode.prev = temp;
        }
    }

    // Update product display with styled "cards"
    static void updateDisplay() {
        panel.removeAll();
        ProductNode temp = head;
        while (temp != null) {
            JPanel card = new JPanel(new BorderLayout());
            card.setBorder(BorderFactory.createCompoundBorder(
                    BorderFactory.createLineBorder(new Color(200, 200, 200), 1, true),
                    BorderFactory.createEmptyBorder(10, 10, 10, 10)
            ));
            card.setBackground(new Color(255, 250, 230)); // soft yellow

            JLabel nameLabel = new JLabel(temp.name, JLabel.CENTER);
            nameLabel.setFont(new Font("SansSerif", Font.BOLD, 18));

            JLabel priceLabel = new JLabel("₹" + temp.price, JLabel.CENTER);
            priceLabel.setFont(new Font("SansSerif", Font.PLAIN, 16));
            priceLabel.setForeground(new Color(33, 150, 243));

            JLabel stockLabel = new JLabel(temp.inStock ? "✅ In Stock" : "❌ Out of Stock", JLabel.CENTER);
            stockLabel.setFont(new Font("SansSerif", Font.PLAIN, 14));
            stockLabel.setForeground(temp.inStock ? new Color(0, 128, 0) : Color.RED);

            card.add(nameLabel, BorderLayout.NORTH);
            card.add(priceLabel, BorderLayout.CENTER);
            card.add(stockLabel, BorderLayout.SOUTH);

            panel.add(card);
            temp = temp.next;
        }
        panel.revalidate();
        panel.repaint();
    }

    // Bubble Sort by price (on linked list)
    static void bubbleSort() {
        if (head == null) return;
        boolean swapped;
        do {
            swapped = false;
            ProductNode curr = head;
            while (curr.next != null) {
                if (curr.price > curr.next.price) {
                    swap(curr, curr.next);
                    swapped = true;
                }
                curr = curr.next;
            }
        } while (swapped);
    }

    // Shuffle using Fisher-Yates (on linked list by swapping data)
    static void shuffle() {
        int size = getSize();
        Random rand = new Random();
        for (int i = size - 1; i > 0; i--) {
            int j = rand.nextInt(i + 1);
            ProductNode nodeI = getNodeAt(i);
            ProductNode nodeJ = getNodeAt(j);
            swap(nodeI, nodeJ);
        }
    }

    // Swap node data (not links)
    static void swap(ProductNode a, ProductNode b) {
        String tempName = a.name;
        boolean tempStock = a.inStock;
        int tempPrice = a.price;

        a.name = b.name;
        a.inStock = b.inStock;
        a.price = b.price;

        b.name = tempName;
        b.inStock = tempStock;
        b.price = tempPrice;
    }

    // Get size of linked list
    static int getSize() {
        int count = 0;
        ProductNode temp = head;
        while (temp != null) {
            count++;
            temp = temp.next;
        }
        return count;
    }

    // Get node at specific index
    static ProductNode getNodeAt(int index) {
        int count = 0;
        ProductNode temp = head;
        while (temp != null) {
            if (count == index) return temp;
            count++;
            temp = temp.next;
        }
        return null;
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