# **Lab 3 Report**
* ## **Part 1**
  * This is my buggy program for this lab
  ```
  public class ArrayExamps{
    // Averages the numbers in the array (takes the mean), but leaves out the
    // lowest number when calculating. Returns 0 if there are no elements or just
    // 1 element in the array
    static double averageWithoutLowest(double[] arr) {
      if(arr.length < 2) { return 0.0; }
      double lowest = arr[0];
      for(double num: arr) {
        if(num < lowest) { lowest = num; }
      }
      double sum = 0;
      for(double num: arr) {
        if(num != lowest) { sum += num; }
      }
      return sum / (arr.length - 1);
    }
  }
  ```
  * A failure-inducing input for the buggy program, as a JUnit test and any associated code
  ```
  import static org.junit.Assert.*;
  import org.junit.*;

  public class ArrayExampsTests {
    @Test
    public void testDupLowest(){
      double[] input1 = {1.0,1.0,1.0,2.0,2.0,3.0,3.0};
      assertEquals(2.0, ArrayExamps.averageWithoutLowest(input1), 0);
    }
  ```
  * An input that doesn't induce a failure, as a JUnit test and any associated code
  ```
    @Test
    public void testDesendingArr(){
      double[] input2 = {1.0, 2.0, 3.0, 4.0};
      assertEquals(3.0, ArrayExamps.averageWithoutLowest(input2), 0);
    }
  }
  ```
  * The symptom, as the output of running the tests
  ![Lab3_Symptom](Lab3_Symptom.png)
  * The bug, as the before-and-after code change required to fix it
    - Before:
      ```
      public class ArrayExamps {
        // Averages the numbers in the array (takes the mean), but leaves out the
        // lowest number when calculating. Returns 0 if there are no elements or just
        // 1 element in the array
        static double averageWithoutLowest(double[] arr) {
          if(arr.length < 2) { return 0.0; }
          double lowest = arr[0];
          for(double num: arr) {
            if(num < lowest) { lowest = num; }
          }
          double sum = 0;
          for(double num: arr) {
            if(num != lowest) { sum += num; }
          }
          return sum / (arr.length - 1);
        }
      }
      ```
    - After:
      ```
      public class ArrayExamps {
        // Averages the numbers in the array (takes the mean), but leaves out the
        // lowest number when calculating. Returns 0 if there are no elements or just
        // 1 element in the array
        static double averageWithoutLowest(double[] arr) {
          if(arr.length < 2) { return 0.0; }
          double lowest = arr[0];
          for(double num: arr) {
            if(num < lowest) { lowest = num; }
          }
          double sum = 0;
          for(double num: arr) {
            sum += num;
          }
          sum -= lowest;
          return sum / (arr.length - 1);
        }
      }
      ```
  * Briefly describe why the fix addresses the issue.
    * The bug in the buggy program lies in the logic of finding the lowest number in the array.
      This loop iterates through each element of the array and updates the lowest variable if the current number is less than the lowest.
      However, when calculating the average later, the code removes all occurrences of the lowest number, which could result in incorrect behavior if there are duplicate lowest numbers in the array.
      By fixing the bug in this way, it can be ensured that only one occurrence of the lowest number is skipped,
      and the calculation of the average is based on the sum and number of the remaining elements correctly.
---
* ## **Part 2**
  ### **The command I choose: `find`**
  ### **Options:**
    ---
    1. `$ find <path> -iname "pattern"`
       * Source: [source_iname](https://docs.rackspace.com/docs/use-the-linux-grep-command)
       * This option of the command `find` is useful since a lot of times we may not remember the exact name of the file we want to search for. It helps find files with approxiamate name as a pattern and case insensitive.
       * Example 1:
         ```
         akapiggy@Yuluns-MacBook-Pro technical % find . -iname "*draft*"
         ./technical/government/Alcohol_Problems/DraftRecom-PDF.txt
         ```
         The -iname option finds the file name in pattern of "draft" under the current directory `technical/`, and output the absolute path to it.
       * Example 2:
         ```
         akapiggy@Yuluns-MacBook-Pro technical % find . -iname "*bill*"
         ./technical/government/Env_Prot_Agen/bill.txt
         ```
         The -iname option finds the file name in pattern of "bill" under the current directory `technical/`, and output the absolute path to it.
    ---
    2. `$ find <path> -type "type"`
       * Source: [source_type](https://docs.rackspace.com/docs/use-the-linux-grep-command)
       * The option `-type` is useful for it can display files, directories, symlinks, named pipes, sockets, and more depends on the type we entered.
       * Example 1:
          ```
          akapiggy@Yuluns-MacBook-Pro technical % find . -type d
          .
          ./government
          ./government/About_LSC
          ./government/Env_Prot_Agen
          ./government/Alcohol_Problems
          ./government/Gen_Account_Office
          ./government/Post_Rate_Comm
          ./government/Media
          ./plos
          ./biomed
          ./911report
          ```
          The -type d shows all the directories under the current directory `technical/`.
       * Example 2:
         ```
         akapiggy@Yuluns-MacBook-Pro technical % find ./911report -type f
         ./911report/chapter-13.4.txt
         ./911report/chapter-13.5.txt
         ./911report/chapter-13.1.txt
         ./911report/chapter-13.2.txt
         ./911report/chapter-13.3.txt
         ./911report/chapter-3.txt
         ./911report/chapter-2.txt
         ./911report/chapter-1.txt
         ./911report/chapter-5.txt
         ./911report/chapter-6.txt
         ./911report/chapter-7.txt
         ./911report/chapter-9.txt
         ./911report/chapter-8.txt
         ./911report/preface.txt
         ./911report/chapter-12.txt
         ./911report/chapter-10.txt
         ./911report/chapter-11.txt
         ```
         The -type f shows all the files under the relative path `./911report` under the current directory `technical/`.
    ---
    3. `$ find <path> -size "size or range"`
       * Source: [source_size](https://linuxhandbook.com/find-command-examples/)
       * The option `-size` can help find big files or small files based on the search performed by the size parameter. This only works with files, not directories.
       * Example 1:
         ```
         akapiggy@Yuluns-MacBook-Pro technical % find . -size +100k
         ./government/About_LSC/commission_report.txt
         ./government/About_LSC/State_Planning_Report.txt
         ./government/Env_Prot_Agen/multi102902.txt
         ./government/Env_Prot_Agen/ctm4-10.txt
         ./government/Env_Prot_Agen/bill.txt
         ./government/Env_Prot_Agen/tech_adden.txt
         ./government/Gen_Account_Office/d0269g.txt
         ./government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
         ./government/Gen_Account_Office/Sept27-2002_d02966.txt
         ./government/Gen_Account_Office/d01376g.txt
         ./government/Gen_Account_Office/Statements_Feb28-1997_volume.txt
         ./government/Gen_Account_Office/pe1019.txt
         ./government/Gen_Account_Office/gg96118.txt
         ./government/Gen_Account_Office/d01591sp.txt
         ./government/Gen_Account_Office/im814.txt
         ./government/Gen_Account_Office/ai9868.txt
         ./government/Gen_Account_Office/May1998_ai98068.txt
         ./government/Gen_Account_Office/d02701.txt
         ./biomed/1471-2105-3-2.txt
         ./911report/chapter-13.4.txt
         ./911report/chapter-13.5.txt
         ./911report/chapter-13.2.txt
         ./911report/chapter-13.3.txt
         ./911report/chapter-3.txt
         ./911report/chapter-1.txt
         ./911report/chapter-6.txt
         ./911report/chapter-7.txt
         ./911report/chapter-9.txt
         ./911report/chapter-12.txt
         ```
         The option `-size + 100k` shows all the files bigger than 100k in the current directory `technical/`.
       * Example 2:
         ```
         akapiggy@Yuluns-MacBook-Pro technical % find . -size +50k -size -55k  
         ./government/Env_Prot_Agen/section-by-section_summary.txt
         ./government/Gen_Account_Office/June30-2000_gg00135r.txt
         ./biomed/1476-069X-2-4.txt
         ./biomed/1471-2253-2-5.txt
         ./biomed/1477-7827-1-13.txt
         ./biomed/1471-2121-3-16.txt
         ./biomed/gb-2002-3-11-research0065.txt
         ./biomed/1471-213X-3-3.txt
         ./biomed/1476-072X-2-4.txt
         ./biomed/gb-2003-4-2-r14.txt
         ./biomed/1471-2091-3-31.txt
         ./biomed/1471-2105-3-14.txt
         ./biomed/gb-2003-4-3-r17.txt
         ./biomed/1471-2121-2-10.txt
         ./biomed/1471-2091-3-14.txt
         ./biomed/gb-2002-3-12-research0088.txt
         ./biomed/1471-2121-3-25.txt
         ./biomed/1471-2210-1-7.txt
         ./biomed/1471-2121-3-30.txt
         ./biomed/gb-2002-3-10-research0052.txt
         ./biomed/gb-2003-4-2-r9.txt
         ./biomed/1471-2091-3-4.txt
         ./biomed/1471-2164-4-4.txt
         ./biomed/1477-7827-1-36.txt
         ./biomed/1471-2199-2-4.txt
         ```
         The option `-size +50k -size -55k` shows all the files bigger than 50k and smaller than 55k in the current directory `technical/`.
    ---
    4. `$ find <path> -maxdepth "levels"` or `$ find <path> -mindepth "levels"`
       * Source: [source_maxdepth](https://www.computerhope.com/unix/ufind.htm)
       * Suppose we have multiple layers of nested subdirectories in a path. The option `-maxdepth` and `mindepth` are useful for it allows us to specify the levels of directories as we want.
       * Example 1:
         ```
         akapiggy@Yuluns-MacBook-Pro technical % find . -maxdepth 1
         .
         ./government
         ./plos
         ./biomed
         ./911report
         akapiggy@Yuluns-MacBook-Pro technical %
         ```
         The option `-maxdepth 1` shows all the files with level 1 in the current directory `technical/`.
       * Example 2:
         ```
         akapiggy@Yuluns-MacBook-Pro technical % find ./government -maxdepth 1
         ./government
         ./government/About_LSC
         ./government/Env_Prot_Agen
         ./government/Alcohol_Problems
         ./government/Gen_Account_Office
         ./government/Post_Rate_Comm
         ./government/Media
         akapiggy@Yuluns-MacBook-Pro technical %
         ```
         The option `-maxdepth 1` shows all the files with level 1 in the relative path `./government`
