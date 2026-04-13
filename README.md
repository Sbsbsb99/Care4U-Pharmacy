import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class Care4uPharmacy extends JFrame implements ActionListener {

    JButton btnAdd, btnRemove, btnView, btnSearch, btnPayment, btnExit;

    static String[] product = {
            "Panadol Actifast","Vit C 1000mg","Fish Oil",
            "Cough Syrup","Sunsilk Shampoo","Dove Body Wash",
            "Colgate Toothpaste","Face Mask",
            "Dettol Sanitizer","Nivea Moisturizer"
    };

    static double[] price = {
            8.50,12.00,25.90,9.80,15.90,
            13.50,6.80,5.50,4.90,14.20
    };

    static int[] stock = {
            10,20,20,10,30,35,10,20,20,10
    };

    public Care4uPharmacy() {

        setTitle("Care4U Pharmacy System");
        setSize(400, 300);
        setLayout(new GridLayout(6, 1));

        btnAdd = new JButton("Add Product");
        btnRemove = new JButton("Remove Product");
        btnView = new JButton("View Product");
        btnSearch = new JButton("Search Product");
        btnPayment = new JButton("Payment");
        btnExit = new JButton("Exit");

        add(btnAdd);
        add(btnRemove);
        add(btnView);
        add(btnSearch);
        add(btnPayment);
        add(btnExit);

        btnAdd.addActionListener(this);
        btnRemove.addActionListener(this);
        btnView.addActionListener(this);
        btnSearch.addActionListener(this);
        btnPayment.addActionListener(this);
        btnExit.addActionListener(this);

        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {

        if (e.getSource() == btnAdd) {
            addProduct();
        } else if (e.getSource() == btnRemove) {
            removeProduct();
        } else if (e.getSource() == btnView) {
            viewProduct();
        } else if (e.getSource() == btnSearch) {
            searchProduct();
        } else if (e.getSource() == btnPayment){
            payment();
        } else {
            System.exit(0);
        }
    }


    void addProduct() {

        String name = JOptionPane.showInputDialog(null,"Enter product name:");
        String id = JOptionPane.showInputDialog(null,"Enter ID:");
        String priceStr = JOptionPane.showInputDialog(null,"Enter price:");
        String quantity = JOptionPane.showInputDialog(null,"Enter quantity:");

        int confirm = JOptionPane.showConfirmDialog(null,
                "Add this product?\n\n" +
                        "ID: " + id +
                        "\nName: " + name +
                        "\nPrice: RM " + priceStr +
                        "\nQty: " + quantity);

        if (confirm == JOptionPane.OK_OPTION) {

            double p = Double.parseDouble(priceStr);
            int q = Integer.parseInt(quantity);


            String[] newProduct = new String[product.length + 1];
            double[] newPrice = new double[price.length + 1];
            int[] newStock = new int[stock.length + 1];

            for (int i = 0; i < product.length; i++) {
                newProduct[i] = product[i];
                newPrice[i] = price[i];
                newStock[i] = stock[i];
            }

            newProduct[product.length] = name;
            newPrice[price.length] = p;
            newStock[stock.length] = q;

            product = newProduct;
            price = newPrice;
            stock = newStock;

            JOptionPane.showMessageDialog(null, "Product added");
        }
    }


    void removeProduct() {

        String name = JOptionPane.showInputDialog("Enter product name:");

        int index = -1;

        for (int i = 0; i < product.length; i++) {
            if (product[i].equalsIgnoreCase(name)) {
                index = i;
                break;
            }
        }

        if (index == -1) {
            JOptionPane.showMessageDialog(null, "Product not found!");
            return;
        }

        int confirm = JOptionPane.showConfirmDialog(null,
                "Are you sure to remove " + name + "?");

        if (confirm == JOptionPane.YES_OPTION) {

            String[] newProduct = new String[product.length - 1];
            double[] newPrice = new double[price.length - 1];
            int[] newStock = new int[stock.length - 1];

            int j = 0;
            for (int i = 0; i < product.length; i++) {
                if (i != index) {
                    newProduct[j] = product[i];
                    newPrice[j] = price[i];
                    newStock[j] = stock[i];
                    j++;
                }
            }

            product = newProduct;
            price = newPrice;
            stock = newStock;

            JOptionPane.showMessageDialog(null, "Product " + name + " Removed");
        }
    }

    void viewProduct() {

        int response = JOptionPane.showConfirmDialog(
                null,
                "Are you sure to view all products?",
                "Confirm",
                JOptionPane.YES_NO_OPTION
        );

        if (response == JOptionPane.YES_OPTION) {

            String output = "ID   Product Name            Price(RM)   Stock\n";

            for (int i = 0; i < product.length; i++) {
                output += (i+1) + ") " + product[i]
                        + "  RM " + price[i]
                        + "  Stock:" + stock[i] + "\n";
            }
            JOptionPane.showMessageDialog(null, output);
        }
    }

    void searchProduct() {

        String name = JOptionPane.showInputDialog("Enter the product Name:");

        if (name == null || name.isEmpty()) {
            JOptionPane.showMessageDialog(null, "Please enter again.");
            return;
        }

        String result = searchProduct(name);
        if (result.isEmpty()) {
            JOptionPane.showMessageDialog(null, "No product found!");
        } else {
            JOptionPane.showMessageDialog(null, "Found:\n" + result);
        }
    }

    public static String searchProduct(String keyword) {

        StringBuilder output = new StringBuilder();
        keyword = keyword.toLowerCase();

        for (int i = 0; i < product.length; i++) {
            if (product[i].toLowerCase().contains(keyword)) {
                output.append(product[i])
                        .append("\nRM:")
                        .append(price[i])
                        .append("\nStock:")
                        .append(stock[i])
                        .append("\n");
            }
        }

        return output.toString();
    }

    void payment(){

        String name = JOptionPane.showInputDialog("Enter product name:");

        int index = -1;
        for (int i = 0; i < product.length; i++) {
            if (product[i].equalsIgnoreCase(name)) {
                index = i;
                break;
            }
        }

        if (index == -1) {
            JOptionPane.showMessageDialog(null, "Product not found");
            return;
        }

        int quantity = Integer.parseInt(
                JOptionPane.showInputDialog(null,"Enter quantity:")
        );

        if (quantity <= 0) {
            JOptionPane.showMessageDialog(null, "Invalid quantity,please enter again");
            return;
        }else{
            JOptionPane.showMessageDialog(null,
                    "Product:" + name + "\nQuantity:" + quantity);
        }

        if (quantity > stock[index]) {
            JOptionPane.showMessageDialog(null, "Sorry , not enough stock");
            return;
        }

        double total = price[index] * quantity;

        int response = JOptionPane.showConfirmDialog(null,
                "Total: RM " + String.format("%.2f", total) +
                        "\nContinue Payment?",
                "Confirm",
                JOptionPane.OK_CANCEL_OPTION);

        if (response == JOptionPane.OK_OPTION) {

            String[] choice = {"Cash", "Card"};
            int pay = JOptionPane.showOptionDialog(null,
                    "Choose the payment method?",
                    "Payment",
                    JOptionPane.DEFAULT_OPTION,
                    JOptionPane.INFORMATION_MESSAGE,
                    null,
                    choice,
                    choice[0]);

            String choose;
            if (pay == 0) {
                choose = "Cash";
            } else {
                choose = "Card";
            }

            stock[index] = stock[index] - quantity;

            JOptionPane.showMessageDialog(null,
                    "Item: " + product[index] +
                            "\nQty: " + quantity+
                            "\nTotal: RM " + String.format("%.2f", total) +
                            "\nPay: " + choose +
                            "\nStock left: " + stock[index]
            );

        } else {
            JOptionPane.showMessageDialog(null, "Payment Cancelled");
        }
    }

    public static void main(String[] args) {
        new Care4uPharmacy();
    }
}
