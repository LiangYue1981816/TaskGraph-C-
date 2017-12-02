TaskGraph��C#�汾ʵ��

һ. ʹ��˵��
1. ��������Ҫ��TaskBase���������Լ��������ಢʵ�����е�ִ�к���
2. ��ʹ��ʱ����������ӵ�TaskGraph��
3. ����TaskGraph��ִ��
4. �ȴ�TaskGraphִ�н��������Ǳ��룩

��. ʹ�ý���
1. TaskGraph���Ŀ���ǽ���Щ�����������������ϵ͵������ֲ��л���������Ϸ�еĸ����߼�Update�ȡ��������������ʱ������Ҳ��ɲ���������ʹ�ö����̵߳������㣬�������غ�Ѱ·�ȡ�
2. ����ִ������֮�������������ϵ����������ӵ�TaskGraph��ͨ��TaskEvent���ú���������ϵ��TaskGraph�ڲ������е��ȣ��������ܲ���ִ�С�

��. ʵ������

class Program
{
	static void Main(string[] args)
	{
		int count = 10;
		Data data = new Data();
		TaskStep0[] step0 = new TaskStep0[count];
		TaskStep1[] step1 = new TaskStep1[count];
		TaskStep2[] step2 = new TaskStep2[count];
		
		for (int index = 0; index < count; index++)
		{
			step0[index] = new TaskStep0(data, index, count);
			step1[index] = new TaskStep1(data, index, count);
			step2[index] = new TaskStep2(data, index, count);
		}
		
		TaskEvent event1 = new TaskEvent();
		TaskEvent event2 = new TaskEvent();
		
		TaskGraph taskGraph = new TaskGraph();
		taskGraph.Create(8);
		
		while (true)
		{
			ConsoleKeyInfo key = Console.ReadKey();
			if (key.Key != ConsoleKey.Escape)
			{
				Console.WriteLine("");
				
				for (int index = 0; index < count; index++)
				{
					taskGraph.Task(step0[index], event1, null);
					taskGraph.Task(step1[index], event2, event1);
					taskGraph.Task(step2[index], null, event2);
				}
				
				taskGraph.Dispatch();
				taskGraph.Wait();
				
				continue;
			}
	
			break;
		}
		
		taskGraph.Destroy();
	}
}