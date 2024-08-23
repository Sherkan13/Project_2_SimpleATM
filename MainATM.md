import Exeptions.SumaNegativa;
import Exeptions.SumaPreaMare;
import Service.AtmService;
import Service.BankAccount;

import java.util.Scanner;

public class AtmMain {

    public AtmMain() {}

    public static void main(String[] args) {
        AtmService atmService = new AtmService();
        Scanner sc = new Scanner(System.in);
        atmService.createAccount("0001", "Ion Popescu", "0000", 101010.0);
        atmService.createAccount("0010", "Vasile Ion", "0001", 5453.0);
        atmService.createAccount("0100", "Ioana Ungureanu", "0010", 12345.0);
        boolean exit = false;
        int optiune;

        while(true) {
            while(!exit) {
                System.out.println("Introduceti ID: ");
                String accountID = sc.nextLine();
                System.out.println("Introduceti PIN: ");
                String pin = sc.nextLine();
                BankAccount account = atmService.authenticate(accountID, pin);
                if (account != null) {
                    System.out.println("Autentificat!");

                    while(!exit) {
                        System.out.println("1. Verificare sold    2. Depunere numerar     3. Retragere numerar    4. Iesire ");
                        optiune = Integer.parseInt(sc.nextLine());

                        try {
                            switch (optiune) {
                                case 1:
                                    account.displayBalance();
                                    break;
                                case 2:
                                    System.out.println("Introduceti suma pe care doriti sa o introduceti: ");
                                    double sumaDepunere = Double.parseDouble(sc.nextLine());
                                    account.deposit(sumaDepunere);
                                    break;
                                case 3:
                                    System.out.println("Introduceti suma pe care ati dori sa o retrageti: ");
                                    double sumaScoatere = Double.parseDouble(sc.nextLine());
                                    account.withdraw(sumaScoatere);
                                    break;
                                case 4:
                                    System.out.println("Ati iesit din aplicatie!");
                                    sc.close();
                                    exit = true;
                                    break;
                                default:
                                    System.out.println("Input eronat!");
                            }
                        } catch (SumaPreaMare | SumaNegativa | IllegalStateException var12) {
                            Exception e = var12;
                            System.out.println(e.getMessage());
                        }
                    }
                } else {
                    System.out.println("Date incorecte sau cont inexistent!");
                }
            }

            return;
        }
    }
}
