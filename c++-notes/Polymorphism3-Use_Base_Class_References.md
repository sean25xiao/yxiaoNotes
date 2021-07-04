# C++ Polymorphism 3 - Poly Using Base Class References

+ 我们也可以使用 Base Class Reference 来做 dynamic polymorphism
+ 当我们想传递一个 object 给一个 function 的时候特别有用，因为传递的时候是通过 Base Class Reference 来传递的，但运行的时候，程序能够区分 Derived Class

```c++
void do_withdraw(Account &account, double amount) {
    account.withdraw(amount);
}

int main() {

    Account a;
    Account &ref = a;
    ref.withdraw(1000);		    // In Account::withdraw

    Trust t;
    Account &ref1 = t;
    ref1.withdraw(1000);        // In Trust::withdraw

    Account a1;
    Savings a2;
    Checking a3;
    Trust a4;
   
    do_withdraw(a1, 1000);       // In Account::withdraw
    do_withdraw(a2, 2000);       // In Savings::withdraw
    do_withdraw(a3, 3000);       // In Checking::withdraw
    do_withdraw(a4,  4000);      // In Trust::withdraw


    return 0;
}
```

