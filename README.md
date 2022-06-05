# C-programming
        #include<stdio.h>
        #include<conio.h>
        #include<stdlib.h>
        #include<string.h>
        #include<time.h>
        void insert_record();
        void append_record();
        void file_open(char *bcode,unsigned long int acno,unsigned long int cdno,char *name,int pwd,float total);
        void display_record();
        //void logfileentry(unsigned long int cdno);
        int search_record(int pwd);
        int search_record1(unsigned long int cdno);
        int search_record2(unsigned long int acno);
        void withdraw(unsigned long int cdno,float amt);
        void deposit(unsigned long int cdno,float amt);
        void deposit1(unsigned long int acno,float amt);
        void update_password(unsigned long int cdno, int pwd);
        void check_balance(unsigned long int cdno);
        void delete_record();
        void view_logfile();
        void view_logfile1();
        char type[10];
        struct user
        {
            unsigned long int acno;
            unsigned long int cdno;
            char name[40];
            char bcode[10];
            int pwd;
            float total;
        };
            struct user u;
        int main()
        {
            char uname[30];
            int pass, ch, ch1, ch2, found = 0, count=0, count1=0;
            float amt;
            unsigned long int cardno, accno;

        mainmenu:

            printf("\n************* Welcome to SDM ATM ********************");
            printf( "\nLogin as :\n\t\t1.Admin\n\t\t2.User\n\t\t3.Exit\n\t\tChoose one : ");
            scanf("%d",&ch);

            switch (ch) {
            case 1:
            rerun:
                printf("\nEnter details to login as Admin\n");
                printf("\nEnter password:");
                scanf("%d",&pass);
                if (pass == 6354) {
                admin:
                    printf("\nWelcome to Admin Menu");
                    printf("\n\t\t1.Add User\n\t\t2.Append user\n\t\t3.View All Users\n\t\t4.Delete user\n\t\t5.display transactions(logfile)\n\t\t6.Exit\n\t\tSelect one : ");
                    scanf("%d",&ch1);
                    switch (ch1) {
                    case 1:
                        insert_record();
                        goto admin;

                    case 2:
                        append_record();
                        goto admin;

                    case 3:
                         display_record();
                        goto admin;

                    case 4:
                        delete_record();
                        goto admin;
                    case 5:
                        view_logfile1();
                        goto admin;
                    case 6:
                        break;
                    }
                }
                else {
                    printf("invalid choice\n");
                    goto rerun;
                }
                goto mainmenu;

            case 2:
              user1:printf("\n Enter details to login as User\n");
                printf("Enter card no. :");
                    scanf("%lu", &cardno);
                    found = search_record1(cardno);
                    if(found)
                    {
                   printf("Enter password:");
                   scanf("%d",&pass);
                   found = search_record(pass);
                   count++;

                if (found) {
                user:
                    printf("\nWelcome to User Menu\n\t\t1.Deposit\n\t\t2.Withdraw\n\t\t3.Money Transfer\n\t\t4.Update Password\n\t\t5.Check Balance\n\t\t6.Exit\n\t\tEnter your choice:");
                    scanf("%d",&ch2);

                    switch (ch2) {
                    case 1:
                        printf("Enter the amount to be deposited:\n");
                        scanf("%f",&amt);
                        deposit(cardno,amt);
                        goto user;
                    case 2:
                        printf("Enter the amount to be withdrawn:\n");
                        scanf("%f",&amt);

                        withdraw(cardno,amt);
                        goto user;
                    case 3:
                        printf("Enter the amount to be transferred:\n");
                        scanf("%f", &amt);
                        withdraw(cardno, amt);
    tfr:                if(count<=3)
                        {
                        printf("Enter the account no. to which you want to transfer:\n");
                        scanf("%lu", &accno);
                        found=search_record2(accno);
                        count1++;
                        if(found)
                            deposit1(accno, amt);
                        else
                        {
                            printf("Invalid acc no.\n");
                            goto tfr;
                        }
                        }
                        else
                        {
                            printf("Too many wrong attempts!!\n");
                            goto user;
                        }
                        goto user;
                    case 4:
                        update_password(cardno, pass);
                        goto user;
                    case 5:
                         check_balance(cardno);
                         goto user;
                    case 6:
                        printf("Thank you");
                        break;
                    }
                }
                else {
                    printf("\n wrong password\n");
                    if(count==3){
                        printf("Many attempts!!!try later\n");
                        goto mainmenu;
                    }else
                        goto user1;
                }
            }
            else
            {
                printf("Card not valid!!\n");
                goto mainmenu;
            }
                goto mainmenu;

            case 3:
                printf("\nThankyou for banking with SDM");
                break;
            }
        return 0;
        }
        void insert_record()
        {
                FILE *f1,*f2;
                f1 = fopen("us.txt","w");
                f2 = fopen("us1.txt","w");

                    printf("Enter the bank code [5 characters]: ");
                    scanf("%s", u.bcode );
                    printf("Enter accountNo [8-digit]: ");
                    scanf("%lu",&u.acno);
                    printf("enter card number [8-digit]:");
                    scanf("%lu",&u.cdno);
                    fflush(stdin);
                    printf("enter Name:");
                    scanf("%s",u.name);
                    printf("enter password [4-digit]:");
                    scanf("%d",&u.pwd);
                    printf("enter total amount:");
                    scanf("%f",&u.total);
                    fwrite(&u,sizeof(u),1,f2);
                    fprintf(f1,"Bankcode \t account_no \t card_no \t Name \t Password \t total");
                    fprintf(f1, "\n%-11s %-12lu %-14lu %-15s %-10d %.2f\n", u.bcode, u.acno, u.cdno,u.name, u.pwd, u.total);
                fclose(f1);
                fclose(f2);
        }
        void append_record()
        {
                FILE *f1,*f2;
                f1 = fopen("us.txt","a");
                f2 = fopen("us1.txt","a");
                    printf("Enter the bank code [5 characters]: ");
                    scanf( "%s", u.bcode );
                    printf("Enter accountNo [8-digit]: ");
                    scanf("%lu",&u.acno);
                    printf("enter card number [8-digit]:");
                    scanf("%lu",&u.cdno);
                    fflush(stdin);
                    printf("enter Name:");
                    scanf("%s",u.name);
                    printf("enter password [4-digit]:");
                    scanf("%d",&u.pwd);
                    printf("enter total amount:");
                    scanf("%f",&u.total);

                    fwrite(&u,sizeof(u),1,f2);
                    fprintf(f1, "\n%-11s %-12lu %-14lu %-15s %-10d %.2f\n", u.bcode, u.acno, u.cdno,u.name, u.pwd, u.total);

                fclose(f1);
                fclose(f2);
        }
        void file_open(char *bcode,unsigned long int acno,unsigned long int cdno,char *name,int pwd,float total){
            FILE *f1;
                f1 = fopen("us.txt","wb+");
                fprintf(f1, "\n%-11s %-12lu %-14lu %-15s %-10d %.2f\n", u.bcode, u.acno, u.cdno,u.name, u.pwd, u.total);
                fclose(f1);
        }

        void display_record()
        {
            FILE *fp;
            fp=fopen("us1.txt","r");

            if(fp==NULL)
            {
                printf("\n\t\tError : Cannot open the File !!!");
                return;
            }

            printf("\n\n\t ****Details of users Are As Follows ****\n");
            printf("\nBank_code\tAccount_no\t\tcard_no\t\t\tName\t\tpassword\ttotal\n\n");

            while(fread(&u,sizeof(u),1,fp)==1)
            {

                printf("%s\t\t%lu\t\t%lu\t\t%s\t\t%d\t\t%0.2f\n",u.bcode,u.acno,u.cdno,u.name,u.pwd,u.total);
            }
            fclose(fp);
        }

        int search_record(int pwd)
        {
            int flag=0;
            FILE *fp;
            fp=fopen("us1.txt","r");
            if(fp==NULL)
            {
                printf("\n\t\tError: Cannot Open the File!!!");
                return 0;
            }
            while(fread(&u,sizeof(u),1,fp)>0 && flag==0)
            {
                if(u.pwd==pwd)
                 flag=1;
            }
            if(flag==0){
            return 0;
            }
            else{
             return 1;
            }
        fclose(fp);
        }

        int search_record1(unsigned long int cdno)
        {
            int flag=0;
            FILE *fp;
            fp=fopen("us1.txt","r");
            if(fp==NULL)
            {
                printf("\n\t\tError: Cannot Open the File!!!");
                return 0;
            }
            while(fread(&u,sizeof(u),1,fp)>0 && flag==0)
            {
                if(u.cdno==cdno)
                 flag=1;
            }
            if(flag==0){
            return 0;
            }
            else{
             return 1;
            }
        fclose(fp);
        }

        int search_record2(unsigned long int acno)
        {
            int flag=0;
            FILE *fp;
            fp=fopen("us1.txt","r");
            if(fp==NULL)
            {
                printf("\n\t\tError: Cannot Open the File!!!");
                return 0;
            }
            while(fread(&u,sizeof(u),1,fp)>0 && flag==0)
            {
                if(u.acno==acno)
                 flag=1;
            }
            if(flag==0){
            return 0;
            }
            else{
             return 1;
            }
        fclose(fp);
        }

        void withdraw(unsigned long int cdno,float amt)
        {
          FILE *fpt,*fpo,*f1,*f2,*f3;

          fpo = fopen("us1.txt", "r");
          fpt = fopen("TempFile.txt", "w");
          while (fread(&u, sizeof(u), 1, fpo))
          {
           if (u.cdno != cdno)
            fwrite(&u, sizeof(u), 1, fpt);
           else
           {
            if(u.total<amt)
              printf("cannot withdraw!!! your bank balance is less than withdrawal amount\n");
            else{
             u.total-=amt;
             printf("amount withdrawn successfully\n");
            }
             fwrite(&u, sizeof(u), 1, fpt);
           }
          }
          fclose(fpo);
          fclose(fpt);
          fpo = fopen("us1.txt", "w");
          fpt = fopen("TempFile.txt", "r");
          f1  =fopen("us.txt","w");
          while (fread(&u, sizeof(u), 1, fpt))
          {
           fwrite(&u, sizeof(u), 1, fpo);
           fprintf(f1, "\n%-11s %-12lu %-14lu %-15s %-10d %.2f\n", u.bcode, u.acno, u.cdno,u.name, u.pwd, u.total);
          }
          fclose(fpo);
          fclose(fpt);
          fclose(f1);
          //creating lock file
          strcpy(type,"debited");
          f1=fopen("us1.txt","r");
          f2=fopen("lft1.txt","a");
          f3=fopen("lft.txt","a");
          while (fread(&u, sizeof(u), 1, f1)){
            if(u.cdno==cdno){
                fwrite(&u,sizeof(u),1,f2);
                fprintf(f3, "\n %-12lu %-14lu %-15s %s\t %-10.2f %.2f\n", u.acno, u.cdno,u.name, type, amt, u.total);
            }
          }
          fclose(f1);
          fclose(f2);
          fclose(f3);

}
        void deposit(unsigned long int cdno,float amt){
             FILE *fpt;
             FILE *fpo,*f1,*f2,*f3;


          fpo = fopen("us1.txt", "r");
          fpt = fopen("TempFile.txt", "w");
          while (fread(&u, sizeof(u), 1, fpo))
          {
           if (u.cdno != cdno)
            fwrite(&u, sizeof(u), 1, fpt);
           else
           {
             u.total+=amt;
             printf("amount deposited successfully\n");
             fwrite(&u, sizeof(u), 1, fpt);
           }
          }
          fclose(fpo);
          fclose(fpt);
          fpo = fopen("us1.txt", "w");
          fpt = fopen("TempFile.txt", "r");
          f1  =fopen("us.txt","w");
          while (fread(&u, sizeof(u), 1, fpt))
          {
           fwrite(&u, sizeof(u), 1, fpo);

          }
          fclose(fpo);
          fclose(fpt);
          fclose(f1);

          //to create lockfile
          strcpy(type,"debited");
          f1=fopen("us1.txt","r");
          f2=fopen("lft1.txt","a");
          f3=fopen("lft.txt","a");
          while (fread(&u, sizeof(u), 1, f1)){
            if(u.cdno==cdno){
                fwrite(&u,sizeof(u),1,f2);
                fprintf(f3, "\n %-12lu %-14lu %-15s %s\t %-10.2f %.2f\n", u.acno, u.cdno,u.name, type, amt, u.total);
            }
          }
          fclose(f1);
          fclose(f2);
          fclose(f3);

}
        void deposit1(unsigned long int acno,float amt){
             FILE *fpt;
             FILE *fpo,*f1,*f2,*f3;


          fpo = fopen("us1.txt", "r");
          fpt = fopen("TempFile.txt", "w");
          while (fread(&u, sizeof(u), 1, fpo))
          {
           if (u.acno != acno)
            fwrite(&u, sizeof(u), 1, fpt);
           else
           {
             u.total+=amt;
             printf("amount transferred successfully\n");
             fwrite(&u, sizeof(u), 1, fpt);
           }
          }
          fclose(fpo);
          fclose(fpt);
          fpo = fopen("us1.txt", "w");
          fpt = fopen("TempFile.txt", "r");
          f1  =fopen("us.txt","w");
          while (fread(&u, sizeof(u), 1, fpt))
          {
           fwrite(&u, sizeof(u), 1, fpo);
           fprintf(f1, "\n%-11s %-12lu %-14lu %-15s %-10d %.2f\n", u.bcode, u.acno, u.cdno,u.name, u.pwd, u.total);
          }
          fclose(fpo);
          fclose(fpt);
          fclose(f1);

          //to create lock file
          strcpy(type,"credited");
          f1=fopen("us1.txt","r");
          f2=fopen("lft1.txt","a");
          f3=fopen("lft.txt","a");
          while (fread(&u, sizeof(u), 1, f1)){
            if(u.acno==acno){
                fwrite(&u,sizeof(u),1,f2);
                fprintf(f3, "\n %-12lu %-14lu %-15s %s\t %-10.2f %.2f\n", u.acno, u.cdno,u.name, type, amt, u.total);
            }
          }
          fclose(f1);
          fclose(f2);
          fclose(f3);


        }

        void update_password(unsigned long int cdno, int pwd){
            FILE *fpt;
             FILE *fpo,*f1;
             int pwd1, pwd2;
        newp:printf("Enter your new password:\n");
             scanf("%d",&pwd1);
             printf("To confirm, re-enter your new password:\n");
             scanf("%d",&pwd2);
            if(pwd1==pwd2)
            {
          fpo = fopen("us1.txt", "r");
          fpt = fopen("TempFile.txt", "w");
          while (fread(&u, sizeof(u), 1, fpo))
          {
            if(u.cdno!=cdno || u.pwd!=pwd)
                fwrite(&u, sizeof(u), 1, fpt);
            else{
                u.pwd=pwd1;
                fwrite(&u, sizeof(u),1,fpt);
            }
          }
          fclose(fpo);
          fclose(fpt);
          fpo = fopen("us1.txt", "w");
          fpt = fopen("TempFile.txt", "r");
          f1  =fopen("us.txt","w");
          while (fread(&u, sizeof(u), 1, fpt))
          {
           fwrite(&u, sizeof(u), 1, fpo);
           fprintf(f1, "\n%-11s %-12lu %-14lu %-15s %-10d %.2f\n", u.bcode, u.acno, u.cdno,u.name, u.pwd, u.total);
          }
          fclose(fpo);
          fclose(fpt);
          fclose(f1);
          printf("password updated successfully\n");
            }
            else
                goto newp;
        }
        void check_balance(unsigned long int cdno){
         FILE *fpo;
         fpo = fopen("us1.txt", "r");
          while (fread(&u, sizeof(u), 1, fpo))
          {
           if (u.cdno == cdno)
            printf("balance is:%f",u.total);
          }
          fclose(fpo);
        }
        void delete_record(){
            FILE *fpo;
         FILE *fpt,*fp1;
         unsigned long int accno;
         printf("Enter the account number you want to delete :");
         scanf("%ul", &accno);
          fpo = fopen("us1.txt", "r");
          fpt = fopen("TempFile.txt", "w");
          while (fread(&u, sizeof(u), 1, fpo))
          {
           if (u.acno != accno)
            fwrite(&u, sizeof(u), 1, fpt);
          }
          fclose(fpo);
          fclose(fpt);
          fpo = fopen("us1.txt", "w");
          fp1=fopen("us.txt","w");
          fpt = fopen("TempFile.txt", "r");
          while (fread(&u, sizeof(u), 1, fpt)){
           fwrite(&u, sizeof(u), 1, fpo);
           fprintf(fp1, "\n%-11s %-12lu %-14lu %-15s %-10d %.2f\n", u.bcode, u.acno, u.cdno,u.name, u.pwd, u.total);
          }
          printf("\nRECORD DELETED\n");
          fclose(fpo);
          fclose(fpt);
          fclose(fp1);
        }
        void view_logfile(){
            FILE *f1;
            f1=fopen("lft1.txt","r");
            while(fread(&u,sizeof(u),1,f1)){
                printf("\n %-12lu %-14lu %-15s debited\t  %.2f\n", u.acno, u.cdno,u.name,  u.total);
            }
            fclose(f1);
        }
        void view_logfile1(){
            FILE *f1;
            f1=fopen("lft1.txt","r");
            char line[100];
            while (fgets(line, 100, f1) != NULL)
        printf("%s", line);

            fclose(f1);
        }
