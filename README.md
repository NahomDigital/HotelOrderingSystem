import java.util.Scanner;
import java.util.ArrayList;
import java.io.FileWriter;
import java.io.IOException;

// ================= ABSTRACTION =================
abstract class Person {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        if (name == null || name.length() < 2) {
            System.out.println("Invalid name");
        } else {
            this.name = name;
        }
    }

    abstract void displayInfo();
}

// ================= INHERITANCE =================
class Customer extends Person {
    private String phone;

    Customer(String name, String phone) {
        setName(name);
        this.phone = phone;
    }

    public String getPhone() {
        return phone;
    }

    void displayInfo() {
        System.out.println("Customer: " + getName() + " | " + phone);
    }
}

// ================= CUSTOM EXCEPTION =================
class InvalidChoiceException extends Exception {
    public InvalidChoiceException(String msg) {
        super(msg);
    }
}

// ================= MAIN CLASS =================
public class FoodOrder {

    private String adminPassword = "9999";

    // MULTI CUSTOMER
    ArrayList<Customer> customers = new ArrayList<>();
    Customer currentCustomer;

    ArrayList<String> orderList = new ArrayList<>();
    ArrayList<Integer> priceList = new ArrayList<>();

    int kitfo = 1000;
    int tibs = 1000;
    int dulet = 400;
    int shiro = 150;
    int beyaynet = 150;
    int kitaFirfir = 200;
    int pasta = 150;
    int burger = 700;
    int chechebsa = 200;
    int genfo = 500;
    int doroWet = 1500;
    int pizza = 500;

    int cocaCola = 50;
    int sprite = 50;
    int pepsi = 50;
    int coffee = 40;
    int tea = 30;

    int ch = 0;
    int quantity = 0;
    static int total = 0;

    Scanner sc = new Scanner(System.in);

    // ================= LOGIN =================
    public void login() {

        System.out.println("================================");
        System.out.println("        WOLLO HOTEL");
        System.out.println("================================");

        System.out.print("Enter Name: ");
        String name = sc.next();

        System.out.print("Enter Phone: ");
        String phone = sc.next();

        // 🔥 PASSWORD VALIDATION
        String pass;
        do {
            System.out.print("Create Password (min 4 chars): ");
            pass = sc.next();
        } while (pass.length() < 4);

        currentCustomer = new Customer(name, phone);
        customers.add(currentCustomer);

        System.out.println("\n✔️ Welcome to Wollo Hotel " + name);
    }

    // ================= MENU =================
    public void displayMenu() {

        System.out.println("\n========= WOLLO HOTEL MENU =========");

        System.out.println("1 Kitfo " + kitfo);
        System.out.println("2 Tibs " + tibs);
        System.out.println("3 Dulet " + dulet);
        System.out.println("4 Shiro " + shiro);
        System.out.println("5 Beyaynet " + beyaynet);
        System.out.println("6 Kita Firfir " + kitaFirfir);
        System.out.println("7 Pasta " + pasta);
        System.out.println("8 Burger " + burger);
        System.out.println("9 Chechebsa " + chechebsa);
        System.out.println("10 Genfo " + genfo);
        System.out.println("11 Doro Wet " + doroWet);
        System.out.println("12 Pizza " + pizza);

        System.out.println("\n13 Coca Cola " + cocaCola);
        System.out.println("14 Sprite " + sprite);
        System.out.println("15 Pepsi " + pepsi);
        System.out.println("16 Coffee " + coffee);
        System.out.println("17 Tea " + tea);

        System.out.println("\n0 Exit / Finish Order");
        System.out.println("18 View Order");
        System.out.println("19 Update Order");
        System.out.println("20 Delete Order");
        System.out.println("21 Search Order");
        System.out.println("22 Admin Panel");
    }

// ================= ADD =================
    public void addItem(String name, int price) {
        System.out.print("Enter quantity: ");
        quantity = sc.nextInt();

        orderList.add(name + " x" + quantity);
        priceList.add(price * quantity);
        total += price * quantity;

        System.out.println("✔️ Added successfully");
    }

    // ================= VIEW =================
    public void viewOrder() {

        System.out.println("\n--- YOUR ORDER ---");

        if (orderList.isEmpty()) {
            System.out.println("No orders yet");
            return;
        }

        for (int i = 0; i < orderList.size(); i++) {
            System.out.println((i + 1) + ". " + orderList.get(i));
        }

        System.out.println("TOTAL: " + total);
    }

    // ================= UPDATE =================
    public void updateOrder() {

        viewOrder();

        System.out.print("Enter item number to update: ");
        int index = sc.nextInt() - 1;

        if (index >= 0 && index < orderList.size()) {

            System.out.print("Enter new quantity: ");
            int newQty = sc.nextInt();

            String item = orderList.get(index).split(" x")[0];
            int oldQty = Integer.parseInt(orderList.get(index).split(" x")[1]);
            int oldPrice = priceList.get(index);

            total -= oldPrice;

            int unitPrice = oldPrice / oldQty;
            int newPrice = unitPrice * newQty;

            // 🔥 REPLACE (FIXED)
            orderList.set(index, item + " x" + newQty);
            priceList.set(index, newPrice);

            total += newPrice;

            System.out.println("✔️ Updated successfully");
        } else {
            System.out.println("Invalid index");
        }
    }

    // ================= DELETE =================
    public void deleteOrder() {

        viewOrder();

        System.out.print("Enter item number to delete: ");
        int index = sc.nextInt() - 1;

        if (index >= 0 && index < orderList.size()) {
            total -= priceList.get(index);
            orderList.remove(index);
            priceList.remove(index);

            System.out.println("✔️ Deleted successfully");
        } else {
            System.out.println("Invalid index");
        }
    }

    // ================= SEARCH =================
    public void searchOrder() {

        if (orderList.isEmpty()) {
            System.out.println("No orders to search");
            return;
        }

        sc.nextLine(); // 🔥 IMPORTANT FIX

        System.out.print("Enter item name: ");
        String name = sc.nextLine().toLowerCase().trim();

        boolean found = false;

        for (String o : orderList) {

            String item = o.split(" x")[0].toLowerCase().trim();

            if (item.contains(name)) {
                System.out.println("✔ Found: " + o);
                found = true;
            }
        }

        if (!found) {
            System.out.println("❌ Item not found");
        }
    }

    // ================= BILL =================
    public void generateBill() {

        System.out.println("\n========= ORDER SUCCESSFUL =========");

        currentCustomer.displayInfo();

        for (String o : orderList) {
            System.out.println(o);
        }

        System.out.println("TOTAL: " + total);

        saveToFile();
    }

    // ================= FILE =================
    public void saveToFile() {
        try {
            FileWriter fw = new FileWriter("daily_sales.txt", true);
            fw.write(currentCustomer.getName() + " | " + total + "\n");
            fw.close();
        } catch (IOException e) {
            System.out.println("File error");
        }
    }

    // ================= ADMIN =================
    public void admin() {

        System.out.print("Admin password: ");
        String p = sc.next();

        if (!p.equals(adminPassword)) {
            System.out.println("Wrong password");
            return;
        }

System.out.println("\n===== ADMIN PANEL =====");
        System.out.println("Total Sales Today: " + total);
        System.out.println("Total Customers: " + customers.size());

        System.out.println("\n--- CUSTOMER LIST ---");

        for (int i = 0; i < customers.size(); i++) {
            System.out.println((i + 1) + ". " +
                    customers.get(i).getName() + " | " +
                    customers.get(i).getPhone());
        }
    }

    // ================= ORDER LOOP =================
    public void order() {

        while (true) {

            try {
                System.out.print("\nChoose: ");
                ch = sc.nextInt();

                switch (ch) {

                    case 1: addItem("Kitfo", kitfo); break;
                    case 2: addItem("Tibs", tibs); break;
                    case 3: addItem("Dulet", dulet); break;
                    case 4: addItem("Shiro", shiro); break;
                    case 5: addItem("Beyaynet", beyaynet); break;
                    case 6: addItem("Kita Firfir", kitaFirfir); break;
                    case 7: addItem("Pasta", pasta); break;
                    case 8: addItem("Burger", burger); break;
                    case 9: addItem("Chechebsa", chechebsa); break;
                    case 10: addItem("Genfo", genfo); break;
                    case 11: addItem("Doro Wet", doroWet); break;
                    case 12: addItem("Pizza", pizza); break;

                    case 13: addItem("Coca Cola", cocaCola); break;
                    case 14: addItem("Sprite", sprite); break;
                    case 15: addItem("Pepsi", pepsi); break;
                    case 16: addItem("Coffee", coffee); break;
                    case 17: addItem("Tea", tea); break;

                    case 0:
                        generateBill();
                        System.out.println("\n✔️ Order Finished Successfully");

                        orderList.clear();
                        priceList.clear();
                        total = 0;

                        System.out.println("\n👉 New Customer Login");
                        login();
                        displayMenu();
                        break;

                    case 18: viewOrder(); break;
                    case 19: updateOrder(); break;
                    case 20: deleteOrder(); break;
                    case 21: searchOrder(); break;
                    case 22: admin(); break;

                    default:
                        throw new InvalidChoiceException("Invalid choice");
                }

            } catch (Exception e) {
                System.out.println("Error: " + e.getMessage());
                sc.nextLine();
            }
        }
    }

    // ================= MAIN =================
    public static void main(String[] args) {
        FoodOrder f = new FoodOrder();
        f.login();
        f.displayMenu();
        f.order();
    }
}
