import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.*;

class Stock {
    String name;
    double price;

    Stock(String name, double price) {
        this.name = name;
        this.price = price;
    }
}

class User {
    double balance = 10000;
    HashMap<String, Integer> portfolio = new HashMap<>();
}

public class StockTradingUI extends JFrame implements ActionListener {

    JTextArea display;
    JTextField stockField, qtyField;
    JButton viewBtn, buyBtn, sellBtn, portfolioBtn;

    ArrayList<Stock> market = new ArrayList<>();
    User user = new User();

    StockTradingUI() {
        setTitle("Stock Trading App");
        setSize(500, 400);
        setLayout(new FlowLayout());
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        // Market Data
        market.add(new Stock("TCS", 3500));
        market.add(new Stock("Infosys", 1500));
        market.add(new Stock("Wipro", 500));

        // UI Components
        display = new JTextArea(10, 40);
        display.setEditable(false);

        stockField = new JTextField(10);
        qtyField = new JTextField(5);

        viewBtn = new JButton("View Market");
        buyBtn = new JButton("Buy");
        sellBtn = new JButton("Sell");
        portfolioBtn = new JButton("Portfolio");

        add(new JLabel("Stock:"));
        add(stockField);
        add(new JLabel("Qty:"));
        add(qtyField);

        add(viewBtn);
        add(buyBtn);
        add(sellBtn);
        add(portfolioBtn);

        add(new JScrollPane(display));

        // Actions
        viewBtn.addActionListener(this);
        buyBtn.addActionListener(this);
        sellBtn.addActionListener(this);
        portfolioBtn.addActionListener(this);

        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {

        if (e.getSource() == viewBtn) {
            display.setText("--- Market ---\n");
            for (Stock s : market) {
                display.append(s.name + " : " + s.price + "\n");
            }
        }

        else if (e.getSource() == buyBtn) {
            String name = stockField.getText();
            int qty = Integer.parseInt(qtyField.getText());

            for (Stock s : market) {
                if (s.name.equalsIgnoreCase(name)) {
                    double cost = s.price * qty;

                    if (user.balance >= cost) {
                        user.balance -= cost;
                        user.portfolio.put(name,
                                user.portfolio.getOrDefault(name, 0) + qty);
                        display.setText("Stock Bought!");
                    } else {
                        display.setText("Insufficient Balance!");
                    }
                }
            }
        }

        else if (e.getSource() == sellBtn) {
            String name = stockField.getText();
            int qty = Integer.parseInt(qtyField.getText());

            if (user.portfolio.containsKey(name) &&
                    user.portfolio.get(name) >= qty) {

                for (Stock s : market) {
                    if (s.name.equalsIgnoreCase(name)) {
                        user.balance += s.price * qty;
                        user.portfolio.put(name,
                                user.portfolio.get(name) - qty);
                        display.setText("Stock Sold!");
                    }
                }
            } else {
                display.setText("Not enough stock!");
            }
        }

        else if (e.getSource() == portfolioBtn) {
            display.setText("--- Portfolio ---\n");
            for (String key : user.portfolio.keySet()) {
                display.append(key + " : " + user.portfolio.get(key) + "\n");
            }
            display.append("Balance: " + user.balance);
        }
    }

    public static void main(String[] args) {
        new StockTradingUI();
    }
}<img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e11307d8-83e8-40ae-8db7-234566987e96" />
# CodeAlpha_Stock_trading_Platform
he Stock Trading Platform is a Java console application that allows users to buy and sell stocks using a virtual balance. It stores data using ArrayList and HashMap and tracks the user’s portfolio. The project demonstrates basic stock trading operations using Object-Oriented Programming concepts in Java.
