Take input of an array without taking length 

package ELc;

import java.util.Arrays;

public class addthelast {
	public static void main(String[] args) {

		String s=IO.readln("Enter the String faormate: ");
		String s1[]=s.split(" ");
		
		
		
		int[] intArray=new int[s.length()];
		
		for(int i=0;i<intArray.length;i++)
		{
			int n=s.charAt(i)-'0';
			intArray[i]=n;
		}
		
		
		
		int c=' ';
		IO.println(c);
		IO.println(Arrays.toString(s1));
		IO.println(Arrays.toString(intArray));
	}
}

