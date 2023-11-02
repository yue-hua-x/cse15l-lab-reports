# part 1: bugs 🐞
bug: `ArrayExamples reversed`  
failure inducing input:
```
  @Test
  public void testReversed1() {
    int[] input1 = {1, 2, 3};
    assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1));
  }
```
non-failure inducing input:
```
  @Test
  public void testReversed2() {
    int[] input1 = { };
    assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
  }
```
symptom:
![image](https://github.com/yue-hua-x/cse15l-lab-reports/assets/146787492/c7923d78-6530-45dc-a4a6-fa2826b5e42b)

bug:
```
//before
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```

```
//after
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
```
# part 2: researching `find`
for all of these examples, i just used the [man page](https://man7.org/linux/man-pages/man1/find.1.html).  
filetree that i'll be using find on:
## option 1: `-ipath pattern`
the `-ipath` option finds all files such that the path of the file matches the pattern passed after `-path`, case insensitive.
```
##finding all files in ./technical/ with "oo" in their path. why? because it looks like a pig nose.
[yuehua@fedora docsearch]$ find ./technical/ -ipath "*oo*" -type f
./technical/government/Env_Prot_Agen/ro_clear_skies_book.txt
./technical/government/Media/AP_LawSchoolDebts.txt
./technical/government/Media/Advocate_for_Poor.txt
./technical/government/Media/Boone_legal_service.txt
./technical/government/Media/FY_04_Budget_Outlook.txt
./technical/government/Media/Firm_to_the_Poor_Needs_Help.txt
./technical/government/Media/Good_guys_reward.txt
./technical/government/Media/Law-school_grads.txt
./technical/government/Media/Law_Schools.txt
./technical/government/Media/Legal_Aid_looks_to_legislators.txt
./technical/government/Media/Legal_services_for_poor.txt
./technical/government/Media/Legal_system_fails_poor.txt
./technical/government/Media/Oregon_Poor.txt
./technical/government/Media/Poor_Lacking_Legal_Aid.txt
./technical/government/Media/Too_Crucial_to_Take_Cut.txt
./technical/government/Media/Unusual_Woodburn.txt
./technical/government/Media/Using_Tech_Tools.txt
```
```
##finding all files with the characters of "amogus" in order :)
[yuehua@fedora docsearch]$ find ./technical/ -ipath "*a*m*o*g*u*s*"
./technical/government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
./technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
./technical/government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
./technical/government/Media/Good_guys_reward.txt
```

## option 2: `-ls`
the `-ls` option prints the files with some additional information, in the format of `ls -dils`.
```
##information about the directories in ./technical/
  1384419      0 drwxr-xr-x   1 yuehua   yuehua         58 Oct 31 11:11 ./technical/
  1384420      0 drwxr-xr-x   1 yuehua   yuehua        474 Oct 31 11:11 ./technical/911report
  1384438      0 drwxr-xr-x   1 yuehua   yuehua      29500 Oct 31 11:11 ./technical/biomed
  1385276      0 drwxr-xr-x   1 yuehua   yuehua        150 Oct 31 11:11 ./technical/government
  1385277      0 drwxr-xr-x   1 yuehua   yuehua        922 Oct 31 11:11 ./technical/government/About_LSC
  1385295      0 drwxr-xr-x   1 yuehua   yuehua        132 Oct 31 11:11 ./technical/government/Alcohol_Problems
  1385300      0 drwxr-xr-x   1 yuehua   yuehua        402 Oct 31 11:11 ./technical/government/Env_Prot_Agen
  1385315      0 drwxr-xr-x   1 yuehua   yuehua       2504 Oct 31 11:11 ./technical/government/Gen_Account_Office
  1385407      0 drwxr-xr-x   1 yuehua   yuehua       6128 Oct 31 11:11 ./technical/government/Media
  1385553      0 drwxr-xr-x   1 yuehua   yuehua        662 Oct 31 11:11 ./technical/government/Post_Rate_Comm
  1385568      0 drwxr-xr-x   1 yuehua   yuehua       9696 Oct 31 11:11 ./technical/plos
```
```
##some extra information about those amogus files
[yuehua@fedora docsearch]$ find ./technical/ -ipath "*a*m*o*g*u*s*" -ls
  1385280     28 -rwxr-xr-x   1 yuehua   yuehua      28121 Oct 31 11:11 ./technical/government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
  1385282      8 -rwxr-xr-x   1 yuehua   yuehua       7083 Oct 31 11:11 ./technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
  1385316    244 -rwxr-xr-x   1 yuehua   yuehua     246218 Oct 31 11:11 ./technical/government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
  1385460      8 -rwxr-xr-x   1 yuehua   yuehua       6946 Oct 31 11:11 ./technical/government/Media/Good_guys_reward.txt
```

## option 3: `-max/mindepth n`
the `-max/mindepth n` options only traverses through a certain number of levels, so that directories deeper than or less deep than a certain depth are not traversed. This can help to filter some files out.
```
##finding directories not immediately in ./technical/
[yuehua@fedora docsearch]$ find ./technical/ -mindepth 2 -type d
./technical/government/About_LSC
./technical/government/Alcohol_Problems
./technical/government/Env_Prot_Agen
./technical/government/Gen_Account_Office
./technical/government/Media
./technical/government/Post_Rate_Comm
```
```
##finding some files with a 7 in the path no deeper than two layers down. output was piped into head because it was very long.
[yuehua@fedora docsearch]$ find ./technical/ -maxdepth 2 -ipath "*7*" -ls | head
  1384434    128 -rwxr-xr-x   1 yuehua   yuehua     128370 Oct 31 11:11 ./technical/911report/chapter-7.txt
  1384439     24 -rwxr-xr-x   1 yuehua   yuehua      24112 Oct 31 11:11 ./technical/biomed/1468-6708-3-1.txt
  1384440     32 -rwxr-xr-x   1 yuehua   yuehua      29585 Oct 31 11:11 ./technical/biomed/1468-6708-3-10.txt
  1384441     20 -rwxr-xr-x   1 yuehua   yuehua      16882 Oct 31 11:11 ./technical/biomed/1468-6708-3-3.txt
  1384442     32 -rwxr-xr-x   1 yuehua   yuehua      31378 Oct 31 11:11 ./technical/biomed/1468-6708-3-4.txt
  1384443     20 -rwxr-xr-x   1 yuehua   yuehua      18114 Oct 31 11:11 ./technical/biomed/1468-6708-3-7.txt
  1384444     24 -rwxr-xr-x   1 yuehua   yuehua      24028 Oct 31 11:11 ./technical/biomed/1471-2091-2-10.txt
  1384445     36 -rwxr-xr-x   1 yuehua   yuehua      35103 Oct 31 11:11 ./technical/biomed/1471-2091-2-11.txt
  1384446     28 -rwxr-xr-x   1 yuehua   yuehua      24851 Oct 31 11:11 ./technical/biomed/1471-2091-2-12.txt
  1384447     28 -rwxr-xr-x   1 yuehua   yuehua      27970 Oct 31 11:11 ./technical/biomed/1471-2091-2-13.txt
```

## option 4: `-newer reference`
the `-newer` option finds all files that were modified after the reference file.
```
##finding all files newer than a random file i chose
[yuehua@fedora docsearch]$ find ./technical/ -newer technical/plos/journal.pbio.0020035.txt -ls | head
  1385568      0 drwxr-xr-x   1 yuehua   yuehua       9696 Oct 31 11:11 ./technical/plos
  1385577     12 -rwxr-xr-x   1 yuehua   yuehua      10153 Oct 31 11:11 ./technical/plos/journal.pbio.0020042.txt
  1385578     20 -rwxr-xr-x   1 yuehua   yuehua      20221 Oct 31 11:11 ./technical/plos/journal.pbio.0020043.txt
  1385579     20 -rwxr-xr-x   1 yuehua   yuehua      19069 Oct 31 11:11 ./technical/plos/journal.pbio.0020046.txt
  1385580      8 -rwxr-xr-x   1 yuehua   yuehua       5847 Oct 31 11:11 ./technical/plos/journal.pbio.0020047.txt
  1385581     16 -rwxr-xr-x   1 yuehua   yuehua      15002 Oct 31 11:11 ./technical/plos/journal.pbio.0020052.txt
  1385582     24 -rwxr-xr-x   1 yuehua   yuehua      21038 Oct 31 11:11 ./technical/plos/journal.pbio.0020053.txt
  1385583     20 -rwxr-xr-x   1 yuehua   yuehua      20355 Oct 31 11:11 ./technical/plos/journal.pbio.0020054.txt
  1385584      8 -rwxr-xr-x   1 yuehua   yuehua       6873 Oct 31 11:11 ./technical/plos/journal.pbio.0020063.txt
  1385585     20 -rwxr-xr-x   1 yuehua   yuehua      16977 Oct 31 11:11 ./technical/plos/journal.pbio.0020064.txt
```
```
##creating a new file, then finding all files newer than it (we should expect nothing).
[yuehua@fedora docsearch]$ touch yeehaw.txt
[yuehua@fedora docsearch]$ find ./technical/ -newer yeehaw.txt 
[yuehua@fedora docsearch]$ 
```
