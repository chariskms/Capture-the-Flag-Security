# ΥΣ13 Προστασία και Ασφάλεια Υπολογιστικών Συστημάτων
## Εαρινό Εξάμηνο 2019-2020
### Διδάσκων: Κώστας Χατζηκοκολάκης
# Εργασία 2

#### Ανακοινώθηκε : 1 Ιουνίου 2020 
#### Προθεσμία παράδοσης: 14 Ιουνίου 2020, 23:59

## OmadaPiraulos:
#### Βασιλοπούλου Θωμαΐς (1115201500016)
#### Γεωργακλή Ελένη (1115201500023)
#### Κατιμερτζής Χαράλαμπος – Μιχαήλ (1115201600062)

## Ερώτημα 1

1. Ακολουθήσαμε τον σύνδεσμο http://2fvhjskjet3n5syd6yfg5lhvwcs62bojmthr35ko5bllr3iqdb4ctdyd.onion/.

2. Ανακαλύψαμε στα στο source-code της σελίδας ένα σχόλιο που μας έκανε redirect στο
   συγκεκριμένο blog: https://blog.0day.rocks/securing-a-web-hidden-service-89d935ba1c1d
   
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_4.png)
   
3. Το άρθρο στο εν λόγω blog περιέγραφε διάφορους τρόπους με τους οποίους μία σελίδα onion μπορεί να προστατεύσει την ιδιωτικότητά της. Στους τρόπους αυτούς περιλαμβάνεται και η απαγόρευση πρόσβασης από τους χρήστες στις σελίδες /server-info και /server-status του Apache. Στη σελίδα των YS13 Fixers όμως, η /server-info ήταν ανοιχτή.

4. Μετά από περιήγηση στο http://2fvhjskjet3n5syd6yfg5lhvwcs62bojmthr35ko5bllr3iqdb4ctdyd.onion/server-info, παρατηρήσαμε την ύπαρξη μιας δεύτερης σελίδας onion στον ίδιο server, για προσωπική χρήση του Fixer, την http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion.

5. Η αναζήτηση στο Διαδίκτυο μας έδωσε την πληροφορία ότι site που τρέχουν σε Apache server διαθέτουν ένα /robots.txt. Έτσι ακολουθώντας το http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/robots.txt.

6. Στο robots, αναγράφεται ότι γίνεται disallow των *.phps σελίδων για τις μηχανές αναζήτησης. Έχοντας περιηγηθεί στο site σε αναζήτηση της λύσης, θυμόμασταν ότι η μοναδική *.php σελίδα που βρήκαμε ήταν η http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/access.php. Ακολουθώντας λοιπόν την πληροφορία αυτή, μεταβήκαμε στο http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/access.phps. 
![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_5.png)

7. Με τον εντοπισμό της σελίδας αυτής, κατέστη σαφές ότι έπρεπε να προσπεράσουμε τις if cases προκειμένου να αποκτήσουμε access σε περαιτέρω πληροφορίες. 
   * Το σχόλιο στην αρχή αναφέρει ότι το username που παίρνει ως GET παράμετρο πρέπει να είναι το 48ο πολλαπλάσιο του 7, να περιέχει το 7 στη δεκαδική του αναπαράσταση και να έχει 7 ψηφία. Με τη βοήθεια του https://math.stackexchange.com/questions/3710141/how-to-find-the-48th-multiple-of-7-that-contains-a-7-in-its-decimal-representati, ανακαλύψαμε ότι είναι το 1337 και προκειμένου να πληροί και τον περιορισμό του 7-ψήφιου αριθμού, μετατρέπεται στο 0001337. 
   * Για το password, εφόσον δεν είχαμε τρόπο να το βρούμε, αποφασίσαμε να ψάξουμε τρόπους να το σπάσουμε. Με λίγη έρευνα, βρήκαμε ότι η 
strcmp επιστρέφει τιμή 0, εάν κάποια από τις δοθείσες τιμές δεν είναι string αλλά array. Έτσι χρησιμοποιώντας αυτό το vulnerability, δώσαμε στην παράμετρο GET την εξής μορφή:
http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/access.php?user=0001337&password[]="".

   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_6.png)

8. Ακολουθώντας την προτροπή της σελιδας, μεταβήκαμε στη σελίδα http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/blogposts7589109238. Με κλικ στο diary, που ήταν το μοναδικό url που είχε η σελίδα, μεταβήκαμε στην http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/blogposts7589109238/blogposts/diary.html. 

9. Στο σημείο αυτό, ήταν ξεκάθαρο ότι θα μπορούσαμε να εκμεταλλευτούμε το directory listing της σελίδας, και ακολουθήσαμε το σύνδεσμο http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/blogposts7589109238/blogposts/.

   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_7.png)

10. Μέσω αναζήτησης στα posts, βρήκαμε το post3.html:
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_8.png)
   Από αυτό είχαμε 2 στοιχεία:
   * Το secret είναι "raccoon".
   * Πρέπει να μετατρέψουμε το visitor number μας σε #100013.
   Έτσι προς το παρόν το μοναδικό clue που μπορούμε να δουλέψουμε είναι η αλλαγή του visitor number πίσω στη σελίδα http://2fvhjskjet3n5syd6yfg5lhvwcs62bojmthr35ko5bllr3iqdb4ctdyd.onion/.
   
11. Έχοντας ήδη αναγνωρίσει ότι το document.cookie περιέχει το prefix "Visitor=", υποπτευθήκαμε ότι το υπόλοιπο string είναι η κρυπτογραφημένη μορφή του visitor number. 
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_9.png) <br>
Αρχικά ψάχνοντας για κρυπτογραφημένα cookie στον Apache βρήκαμε σε αυτή την σελίδα https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwj46YObvYHqAhX8QEEAHR9WBB8QFjAAegQIAhAB&url=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F32589998%2Fapache-cookie-decrypt-flask-session&usg=AOvVaw0HZBwP3iQGKjV45yztef7y ότι το cookie είναι base64 encoded. Σε μια πρώτη προσπάθεια αποκρυπτογράφησης βγήκε ο προϋπάρχων visitor number στην αρχή, ακολουθούμενος από ένα ακόμα κρυπτογραφημένο string. Το νούμερο που υπήρχε σε αυτή την θέση αντικατστάθηκε από το 100013 και κρυπτογραφήσαμε ξανά με base64 το cookie, όμως στην δοκιμασία αυτή, έβγαλε error "bad sha256..." . Έτσι, μετά από αποκρυπτογράφηση, καταλάβαμε ότι το υπόλοιπο string ήταν ο ίδιος αριθμός κρυπτογραφημένος σε sha256. Αυτή την φορά ακολουθήσαμε την σωστή διαδικασία και μας επιτράπει η είσοδος στην επόμενη σελίδα. <br>
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_11.png)
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_12.png)

12. Ανακατευθυνομένοι έτσι στη σωστή σελίδα, ανακαλύψαμε τα logs που αναφέρονταν νωρίτερα και γνωρίζουμε ότι ανήκουν στο Γιώργο. 
![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_13.png)
Με τη βοήθεια του παραπάνω παραδείγματος, καταλάβαμε ότι πρέπει να ανακαλύψουμε την ημερομηνία κρυπτογράφησης. Το secret (στο παράδειγμα cement) το ανακαλύψαμε ήδη στο βήμα 10, ως raccoon. Προκειμένου λοιπόν να βρούμε τη σωστή ημερομηνία, δημιουργήσαμε ένα script που με brute force, δοκιμάζει ημερομηνίες ως prefix, κωδικοποιημένες κατάλληλα, μέχρι να βρει τη 1 που ταιριάζει με το δοθέν passphrase.key και άρα το log που μας ενδιαφέρει. Το script σε python:
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_14.png)
, το οποίο μας έδωσε και το επιθυμητό κλειδί. 

13. Με τη χρήση του gpg αποκρυπτογραφήσαμε τις συνομιλίες του Γιώργου:
![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_10.png)
και βρήκαμε τον κωδικό 2355437c5f30fd2390a314b7d52fb3d24583ef97, που θεωρητικά δίνει την τοποθεσία του Γιώργου.

14. Καταλήξαμε ότι ο κωδικός αυτός είναι από ένα commit hash στο git (οι συνομιλίες του Γιώργου ήταν κατατοπιστικές).
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_15.png)<br>
   Εκτελώντας τα βήματα που παρατέθηκαν, καταλήξαμε στις συντεταγμένες:
   **x= 47.77688064386404676504**
   **y= 4.41200375698630469266**
   
15.  Αναζητώντας τις συντεταγμένες, αποφανθήκαμε ότι ο Γιώργος βρίσκεται στη Γαλλία:
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_16.png)
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_17.png)

   
   
## Ερώτημα 2

1. Συνεχίζοντας από το βήμα 8 του προηγούμενου ερωτήματος και ερευνώντας περαιτέρω το blog της Υβόννης, είδαμε το http://jt4grrjwzyz3pjkylwfau5xnjaj23vxmhskqaeyfhrfylelw4hvxcuyd.onion/blogposts7589109238/blogposts/diary2.html. Απο το σημείο αυτό, καταλάβαμε ότι πρέπει να εκπαιδευτούμε με τον pico server.

2. Αφού τον στήσαμε σε δικό μας μηχάνημα και σχολιάσαμε τη read-file από το δοθέν παράδειγμα, τρέξαμε το server. Εκεί, το compilation μας έδωσε μια πολύτιμη πληροφορία. 
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_18.png)
   
   Το μήνυμα αυτό ουσιαστικά δηλώνει, ότι δε συμπεριλάβαμε τύπο μεταβλητής στην εκτύπωση, αλλά αντ' αυτού εκτυπώνουμε αυθαίρετα τη μεταβλητή. Αυτό μας έδωσε την ιδέα ότι μπορούμε να εκτυπώσουμε σε αντίστοιχο server τα περιεχόμενα της στοίβας.
   
3. Το link http://4tpgiulwmoz4sphv.onion/ στο diary2, παράγει ένα prompt που ζητάει όνομα χρήστη και κωδικό. Εκμεταλλευόμενοι το βήμα 2, δοκιμάσαμε επαναληπτικά να δίνουμε τιμή στο username "%x %s", με σταδιακά αυξανόμενα %x. Όταν δώσαμε την τιμή "%x %x %x %x %x %x %s" πήραμε το μήνυμα:
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_19.png)

4. Αφού πλέον είχαμε username: admin και ένα κωδικοποιημένο password σε md5, κάτι που καταλάβαμε από τον κώδικα του pico server. Έτσι αποκρυπτογραφήσαμε το hash:
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_20.png)
   
5. Εισάγοντας τα σωστά πλεόν credentials ανακατευθυνθήκαμε στη σελίδα:
   ![alt text](https://github.com/chatziko-ys13/2020-project-2-omadapiraulos/blob/master/screenshots/Screenshot_21.png)<br>
   στην οποία φαίνεται ότι αυτό που λείπει από το Plan X είναι ένας **solar wind analyzer**.


