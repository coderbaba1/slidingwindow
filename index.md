#SERVER


import java.io.*;
import java.net.*;
public class SWreciever19 {
 public static void main(String args[]) throws IOException {
  int RWS = 8, LAF = 0, LAR = 0;
  InetAddress obj = InetAddress.getLocalHost();
  Socket s = new Socket(obj, 589);
  BufferedReader dis = new BufferedReader(new InputStreamReader(System.in));
  PrintStream p = new PrintStream(s.getOutputStream());
  while (true) {
   int i = 0;
   while (i < RWS) {
    String t;
    if (LAF - LAR <= RWS) {
     t = dis.readLine();
     if (t.equals("QUIT")) System.exit(0);
     System.out.println("received" + t + "Successfully");
     LAR++;
     p.println("Acknowledgement for" + t);
     LAF++;
    }
    i++;
   }
   System.out.println();
  }
 }
}

##CLIENT

import java.io.*;
import java.net.*;
public class SWsender19 {
 public static void main(String args[]) throws IOException {
  int SWS = 8;
  int LAR = 0;
  int LFS = 0;
  ServerSocket ss = new ServerSocket(5277);
  Socket s = ss.accept();
  System.out.println("Type your message >>>>");
  BufferedReader dis = new BufferedReader(new InputStreamReader(s.getInputStream()));
  PrintStream p = new PrintStream(s.getOutputStream());
  while (true) {
   int i = 0, j = 0;
   BufferedReader br1 = new BufferedReader(new InputStreamReader(System.in));
   String t = br1.readLine();
   if (t.trim().toLowerCase().equals("quit")) {
    p.println(t);
    System.exit(0);
   }
   char c[] = new char[100];
   c = t.toCharArray();
   int sent = 1;
   while (i < t.length()) {
    while (i < t.length() && i < SWS * sent) {
     if (LFS - LAR <= SWS) {
      p.println(c[i]);
      LFS++;
      System.out.println("sent=" + c[i++] + "Successfully");
     }
    }
    while (j < t.length() && j < SWS * sent) {
     String t1 = dis.readLine();
     System.out.println(t1);
     j++;
     LAR++;
    }
    sent++;
    System.out.println();
   }
  }
 }
