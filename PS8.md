# MIT-CS-PS8-V2
# -*- coding: utf-8 -*-
# 6.00 Problem Set 8
#
# Intelligent Course Advisor
#
# Name:
# Collaborators:
# Time:
#

import time

SUBJECT_FILENAME = "subjects.txt"
VALUE, WORK = 0, 1

#
# Problem 1: Building A Subject Dictionary
#
def loadSubjects(filename):
    """
    Returns a dictionary mapping subject name to (value, work), where the name
    is a string and the value and work are integers. The subject information is
    read from the file named by the string filename. Each line of the file
    contains a string of the form "name,value,work".

    returns: dictionary mapping subject name to (value, work)
    """
    # TODO: Instead of printing each line, modify the above to parse the name,
    # value, and work of each subject and create a dictionary mapping the name
    # to the (value, work).
    
    # The following sample code reads lines from the specified file and prints
    # each one.
    inputFile = open(filename)
    subjects = {}
    for line in inputFile:
        templine = []
        templine = line.split(',')
        #print templine#.strip().lower()
        #print templine[2].strip().lower()
        subjects[templine[0]] = (int(templine[1]), int(templine[2].strip().lower()))
        #print line.split(',')
        #print type(line.split(','))
        #print line.split[1]
        #line.split(',')
    #print subjects
    return subjects

def testLoadSubjects():
    loadSubjects("short_subjects.txt")


def printSubjects(subjects):
    """
    Prints a string containing name, value, and work of each subject in
    the dictionary of subjects and total value and work of all subjects
    """
    totalVal, totalWork = 0,0
    if len(subjects) == 0:
        return 'Empty SubjectList'
    res = 'Course\tValue\tWork\n======\t====\t=====\n'
    subNames = subjects.keys()
    #subNames.sort()
    for s in subNames:
        val = subjects[s][0]
        work = subjects[s][1]
        res = res + s + '\t' + str(val) + '\t' + str(work) + '\n'
        totalVal += val
        totalWork += work
    res = res + '\nTotal Value:\t' + str(totalVal) +'\n'
    res = res + 'Total Work:\t' + str(totalWork) + '\n'
    print res

#testsubjects = loadSubjects("subjects.txt")
#print testsubjects
#print len(testsubjects)
#printSubjects(testsubjects)

def cmpValue(subInfo1, subInfo2):
    """
    Returns True if value in (value, work) tuple subInfo1 is GREATER than
    value in (value, work) tuple in subInfo2
    """
    val1 = subInfo1[0]
    val2 = subInfo2[0]
    return  val1 > val2

def cmpWork(subInfo1, subInfo2):
    """
    Returns True if work in (value, work) tuple subInfo1 is LESS than than work
    in (value, work) tuple in subInfo2
    """
    work1 = subInfo1[1]
    work2 = subInfo2[1]
    return  work1 < work2

def cmpRatio(subInfo1, subInfo2):
    """
    Returns True if value/work in (value, work) tuple subInfo1 is 
    GREATER than value/work in (value, work) tuple in subInfo2
    """
    val1 = subInfo1[0]
    val2 = subInfo2[0]
    work1 = subInfo1[1]
    work2 = subInfo2[1]
    return float(val1) / work1 > float(val2) / work2

##testsubjects = loadSubjects("short_subjects.txt")
##printSubjects(testsubjects)
##print cmpValue(testsubjects['2.00'],testsubjects['2.01'])
##print cmpWork(testsubjects['2.00'],testsubjects['2.01'])
##print cmpRatio(testsubjects['2.00'],testsubjects['2.01'])

#
# Problem 2: Subject Selection By Greedy Optimization
#
def greedyAdvisor(subjects, maxWork, comparator):
    """
    Returns a dictionary mapping subject name to (value, work) which includes
    subjects selected by the algorithm, such that the total work of subjects in
    the dictionary is not greater than maxWork.  The subjects are chosen using
    a greedy algorithm.  The subjects dictionary should not be mutated.

    subjects: dictionary mapping subject name to (value, work)
    maxWork: int >= 0
    comparator: function taking two tuples and returning a bool
    returns: dictionary mapping subject name to (value, work)
    """

    workdone = 0
    keyslist = sorted(subjects.keys())
    #print keyslist
    looptracker = 0
    subjectchoice = {}
    chooser = 0
    newchooser = 0
    #print subjects.keys()
    #print 'proof'+str(len(subjects))
    while subjects[keyslist[chooser]][WORK]>maxWork:
        #print 'stuck'
        chooser +=1
        #print chooser
        newchooser = chooser
        #print 'chooser',str(chooser)
    #print 'chooserafterloop',str(chooser)
    while looptracker < len(subjects):
        #print looptracker
##        print len(keyslist)
##        print 'chooser =', str(chooser)
        chooser = newchooser
        for i in range(len(keyslist)):
            #print 'lenkeyslist'+str(len(keyslist))
            #print 'i =', str(i)
            #print sorted(subjects.keys(i),subjects.keys(i+1))
            #General Scheme: sort through and determine the best on each pass. add entry to "choice" dictionary.
            #Delete key from keyslist when a new choice is made, and add the amount of work to workdone.
            #print comparator(subjects[keyslist[i]], subjects[keyslist[chooser]])
            if (comparator(subjects[keyslist[i]], subjects[keyslist[chooser]]) == True) and (subjects[keyslist[i]][WORK]<=maxWork):
                chooser = i
                #print subjects[keyslist[chooser]][0]
            #print chooser
        #print 'value for subjects choice 0:', str(subjects[keyslist[chooser]][VALUE])
        #print 'work for subjects choice 0:',str(subjects[keyslist[chooser]][WORK])
        if subjects[keyslist[chooser]][1] + workdone < maxWork:
            #print subjects[keyslist[chooser]][1]
            workdone += (subjects[keyslist[chooser]][1])
            #print workdone
            subjectchoice[keyslist[chooser]]=subjects[keyslist[chooser]]
            #print subjectchoice
            looptracker +=1
            #print looptracker
            del keyslist[chooser]
        else:
            return subjectchoice
    #print workdone
    return subjectchoice


#Note from Andrew: The following block is all of my test code for greedy advisor
##testsubjects = loadSubjects("short_subjects.txt")
##testsubjectsfull = loadSubjects("subjects.txt")
####printSubjects(testsubjects)
##printSubjects(greedyAdvisor(testsubjects, 15, cmpValue))
##print 'Using full subjects file, cmpValue, and max work of 150:'
##printSubjects(greedyAdvisor(testsubjectsfull, 150, cmpValue))
##print 'Using full subjects file, cmpWork, and max work of 150:'
##printSubjects(greedyAdvisor(testsubjectsfull, 150, cmpWork))
##print 'Using full subjects file, cmpRatio, and max work of 15:'
##printSubjects(greedyAdvisor(testsubjectsfull, 15, cmpRatio))
######print cmpWork(testsubjects['2.00'],testsubjects['2.01'])
####print cmpRatio(testsubjects['2.00'],testsubjects['2.01'])

        

def bruteForceAdvisor(subjects, maxWork):
#Note from Andrew: this was already written
    """
    Returns a dictionary mapping subject name to (value, work), which
    represents the globally optimal selection of subjects using a brute force
    algorithm.

    subjects: dictionary mapping subject name to (value, work)
    maxWork: int >= 0
    returns: dictionary mapping subject name to (value, work)
    """
    nameList = subjects.keys()
    tupleList = subjects.values()
    bestSubset, bestSubsetValue = \
            bruteForceAdvisorHelper(tupleList, maxWork, 0, None, None, [], 0, 0)
    outputSubjects = {}
    for i in bestSubset:
        outputSubjects[nameList[i]] = tupleList[i]
    return outputSubjects

def bruteForceAdvisorHelper(subjects, maxWork, i, bestSubset, bestSubsetValue,
                            subset, subsetValue, subsetWork):
#Note from Andrew: this function was already entirely written
    # Hit the end of the list.
    #print 'BFAH called with i =',str(i),'subset work =',subsetWork
    if i >= len(subjects):
        if bestSubset == None or subsetValue > bestSubsetValue:
            # Found a new best.
            return subset[:], subsetValue
        else:
            # Keep the current best.
            return bestSubset, bestSubsetValue
    else:
        s = subjects[i]
        # Try including subjects[i] in the current working subset.
        print 'subset work =',str(subsetWork)
        if subsetWork + s[WORK] <= maxWork:
            subset.append(i)
            bestSubset, bestSubsetValue = bruteForceAdvisorHelper(subjects,
                    maxWork, i+1, bestSubset, bestSubsetValue, subset,
                    subsetValue + s[VALUE], subsetWork + s[WORK])
##            print 'new'
##            print subset
            subset.pop()
##            print subset
            #print bestSubset
        bestSubset, bestSubsetValue = bruteForceAdvisorHelper(subjects,
                maxWork, i+1, bestSubset, bestSubsetValue, subset,
                subsetValue, subsetWork)
        #print bestSubset, bestSubsetValue
        return bestSubset, bestSubsetValue



#
# Problem 3: Subject Selection By Brute Force
#

def loadSubjectsLength(n):
#This was not a problem requirement, I thought it would be useful for testing
    """
    loads the file "subjects.txt" and pares it down to "n" number of dictionary entries
    """
    subjects = loadSubjects("subjects.txt")
    subjectkeys = subjects.keys()
    newsubjects = {}
    for i in range (n):
        newsubjects[subjectkeys[i]] = subjects[subjectkeys[i]]
    return newsubjects

##printSubjects(loadSubjectsLength(5))
##printSubjects(loadSubjectsLength(15))
##printSubjects(loadSubjectsLength(20))

def bruteForceTime():
    """
    Runs tests on bruteForceAdvisor and measures the time required to compute
    an answer.
    """
    # TODO... (function written by Andrew)


    testsubjects = loadSubjectsLength(5)
    print 'Testing bruteForceAdvisor with 5 subjects and maxWork of 15'
    start_time = time.time()
    printSubjects(bruteForceAdvisor (testsubjects, 15))
    end_time=time.time()
    bruteTime5= (end_time-start_time)
    print 'Time required was ', str(bruteTime5)

    #testsubjects = loadSubjectsLength(5)
    #printSubjects(testsubjects)
    print 'Testing greedyAdvisor with 5 subjects and maxWork of 15, comparing best value'
    start_time = time.time()
    printSubjects(greedyAdvisor (testsubjects, 15, cmpValue))
    end_time=time.time()
    greedyTime5= (end_time-start_time)
    print 'Time required was ', str(greedyTime5)

    testsubjects = loadSubjectsLength(20)
    print 'Testing bruteForceAdvisor with 20 subjects and maxWork of 15'
    start_time = time.time()
    printSubjects(bruteForceAdvisor (testsubjects, 15))
    end_time=time.time()
    bruteTime20= (end_time-start_time)
    print 'Time required was ', str(bruteTime20)

    print 'Testing greedyAdvisor with 20 subjects and maxWork of 15, comparing best value'
    start_time = time.time()
    printSubjects(greedyAdvisor (testsubjects, 15, cmpValue))
    end_time=time.time()
    greedyTime20= (end_time-start_time)
    print 'Time required was ', str(greedyTime20)

    
    testsubjects = loadSubjectsLength(150)
    printSubjects (testsubjects)
    print 'Testing bruteForceAdvisor with 150 subjects and maxWork of 15'
    start_time = time.time()
    printSubjects(bruteForceAdvisor (testsubjects, 15))
    end_time=time.time()
    bruteTime150= (end_time-start_time)
    print 'Time required was ', str(bruteTime150)

    print 'Testing greedyAdvisor with 150 subjects and maxWork of 15, comparing best value'
    start_time = time.time()
    printSubjects(greedyAdvisor (testsubjects, 15, cmpValue))
    end_time=time.time()
    greedyTime150= (end_time-start_time)
    print 'Time required was ', str(greedyTime150)

    

# Problem 3 Observations
# ======================
#
# TODO: write here your observations regarding bruteForceTime's performance


#note from Andrew: following block is test code for brute force advisor
##testsubjects = loadSubjects("short_subjects.txt")
##printSubjects(testsubjects)
##testResult = bruteForceAdvisor (testsubjects, 10)
##printSubjects(testResult)
##printSubjects(greedyAdvisor(testsubjects, 10, cmpValue))

#bruteForceTime()

##testsubjects = loadSubjectsLength(10)
##print 'Testing bruteForceAdvisor with 10 subjects and maxWork of 15'
##start_time = time.time()
##printSubjects(bruteForceAdvisor (testsubjects, 15))
##end_time=time.time()
##bruteTime5= (end_time-start_time)
##print 'Time required was ', str(bruteTime5)


#
# Problem 4: Subject Selection By Dynamic Programming
#
def dpAdvisor(subjects, maxWork):
    """
    Returns a dictionary mapping subject name to (value, work) that contains a
    set of subjects that provides the maximum value without exceeding maxWork.

    subjects: dictionary mapping subject name to (value, work)
    maxWork: int >= 0
    returns: dictionary mapping subject name to (value, work)
    """
    # TODO...

    nameList = subjects.keys()
    tupleList = subjects.values()
    m = {}
    bestSubset, bestSubsetValue = \
            dpAdvisorHelper(tupleList, maxWork, 0, None, None, [], 0, 0, m)
    outputSubjects = {}
    for i in bestSubset:
        outputSubjects[nameList[i]] = tupleList[i]
    return outputSubjects

def dpAdvisorHelper(subjects, maxWork, i, bestSubset, bestSubsetValue,
                            subset, subsetValue, subsetWork, m):

    
    try: return m[(i, subsetWork)]
    except KeyError:
    # Hit the end of the list.
        if i >= len(subjects):
            if bestSubset == None or subsetValue > bestSubsetValue:
                # Found a new best.
                m[(i-1, subsetWork)] = (subset[:], subsetValue)
                #print 'opt 1'
                #print m.keys()
                return subset[:], subsetValue
            else:
                # Keep the current best.
                m[(i-1, subsetWork)] = (bestSubset[:], bestSubsetValue)
                #print 'opt 2'
                #print m.keys()
                return bestSubset, bestSubsetValue
        else:
            s = subjects[i]
            # Try including subjects[i] in the current working subset.
            print 'subset work =',str(subsetWork)
            if subsetWork + s[WORK] <= maxWork:
                subset.append(i)
                bestSubset, bestSubsetValue = dpAdvisorHelper(subjects,
                        maxWork, i+1, bestSubset, bestSubsetValue, subset,
                        subsetValue + s[VALUE], subsetWork + s[WORK], m)
                #m[(i+1, subsetWork+s[WORK])] = (bestSubset[:], bestSubsetValue)
                subset.pop()
            bestSubset, bestSubsetValue = dpAdvisorHelper(subjects,
                    maxWork, i+1, bestSubset, bestSubsetValue, subset,
                    subsetValue, subsetWork, m)
            #print bestSubset, bestSubsetValue
            m[(i, subsetWork)] = (bestSubset, bestSubsetValue)
            #print 'opt 3'
            #print m.keys()
            return bestSubset, bestSubsetValue

testsubjects = loadSubjectsLength(4)
printSubjects(testsubjects)
print 'Testing bruteForceAdvisor with 10 subjects and maxWork of 15'
start_time = time.time()
printSubjects(bruteForceAdvisor (testsubjects, 5))
end_time=time.time()
bruteTime5= (end_time-start_time)
print 'Time required was ', str(bruteTime5)

#testsubjects = loadSubjectsLength(10)
print 'Testing dpAdvisor with 5 subjects and maxWork of 15'
start_time = time.time()
printSubjects(dpAdvisor (testsubjects, 5))
end_time=time.time()
dptime5= (end_time-start_time)
print 'Time required was ', str(dptime5)

#
# Problem 5: Performance Comparison
#
def dpTime():
    """
    Runs tests on dpAdvisor and measures the time required to compute an
    answer.
    """
    # TODO...

# Problem 5 Observations
# ======================
#
# TODO: write here your observations regarding dpAdvisor's performance and
# how its performance compares to that of bruteForceAdvisor.

