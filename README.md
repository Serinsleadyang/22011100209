import java.util.Arrays;
import java.util.Scanner;

// 进程类
class Process {
    int pid;  // 进程的标识符
    int arrivalTime;  // 到达时间
    int burstTime;  // 执行时间

    public Process(int pid, int arrivalTime, int burstTime) {
        this.pid = pid;
        this.arrivalTime = arrivalTime;
        this.burstTime = burstTime;
    }
}

public class SJFScheduling {

    // 比较函数用于排序（根据进程的执行时间）
    static class ProcessComparator implements Comparator<Process> {
        public int compare(Process a, Process b) {
            return a.burstTime - b.burstTime;
        }
    }

    // 短作业优先调度算法函数
    static float SJFScheduling(Process[] processes) {
        Arrays.sort(processes, new ProcessComparator());
        int completionTime = 0;
        float waitingTime = 0;
        for (Process process : processes) {
            if (completionTime > process.arrivalTime) {
                waitingTime += completionTime - process.arrivalTime;
            }
            completionTime += process.burstTime;
            System.out.printf("进程ID为 %d，完成时间为 %d，等待时间为 %.2f\n", process.pid, completionTime, waitingTime);
        }
        float avgWaitingTime = waitingTime / processes.length;
        return avgWaitingTime;
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("请输入进程数量：");
        int numProcesses = scanner.nextInt();
        Process[] processes = new Process[numProcesses];
        for (int i = 0; i < numProcesses; i++) {
            System.out.println("进程ID为 " + (i + 1));
            System.out.print("请输入到达时间：");
            int arrivalTime = scanner.nextInt();
            System.out.print("请输入执行时间：");
            int burstTime = scanner.nextInt();
            processes[i] = new Process(i + 1, arrivalTime, burstTime);
        }
        float avgWaitingTime = SJFScheduling(processes);
        System.out.printf("平均等待时间为：%.2f\n", avgWaitingTime);
    }
}
