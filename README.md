1. Вывести первые 3 элемента массива (Вывести первые 3 элемента массива используя цикл for).
Ответ:

	public static void main(String[] args) {
		int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;

		for ( int i = 0; i < nums.length-4; i++)
		{
			System.out.println(nums[i]);
		}
	}


2. Вывести первую половину массива (Вывести первую первую половину элементов массива при помощи цикла for). Реализация задачи должна работать при любом чётном количестве элементов.

Ответ:

public class Main {

	public static void main(String[] args) {
		int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;
		for ( int i = 0; i < nums.length/2; i++)
		{
			System.out.println(nums[i]);
		}
	}
}

3. Вывести вторую половину массива (Вывести вторую половину элементов массива при помощи цикла for). Реализация задачи должна работать при любом чётном количестве элементов.

Ответ:

public class Main {

   public static void main(String[] args) {
   
      int[] nums = {7, -3, 9, -11, 18, 99, 2, 11};;
      for ( int i = nums.length/2; i < nums.length;  i++)
      {
         System.out.println(nums[i]);
      }
   }
}

Вывести все элементы кроме первого и последнего.

Вывести последние 3 элемента массива (Для массива [7, -3, 9, -11, 18, 99, 2, 11] вывод должен быть таким: 99, 2, 11)

Вывести чётные элементы массива по порядку (Вывести только чётные элементы массива по порядку (каждый второй элемент). Необходимо будет вывести второй, четвёртый, шестой и т.д. элементы. Позиции (индексы) для необходимых элементов: 1, 3, 5...n (постоянное увеличение на 2).

Вывести количество положительных и отрицательных элементов (Необходимо определить количество положительных и отрицательных элементов в массиве и вывести его). В реализации задачи понадобится определить 2 переменные для хранения количества элементов: Одна переменная для хранения количества положительных элементов, допустим positiveCount. Вторая для хранения количества отрицательных элементов, допустим negativeCount. Названия переменных можно выбирать на своё усмотрение.

Вывести элементы массива которые больше предыдущего (Необходимо вывести все элементы массива которые больше чем впереди стоящий). Для массива [7, -3, 9, -11, 18, 99, 2, 11] вывод должен быть таким: 9, 18, 99, 11.

Найти максимальный и минимальный элементы массива (Необходимо определить максимальный и минимальный элементы в массиве и вывести их).


