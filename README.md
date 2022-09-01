# TinyXMLCodeGenerator
A generator that parses annotated C++ classes and generates read/write code for XML files using the TinyXML lib

This code is based on the NANDRAD/VICUS code generator from [SIM-VICUS](https://github.com/ghorwin/SIM-VICUS).

## Usage:

### Annotate classes

```c++
/*! Test class to check correct functionality of the TinyXML code generator.
    \note This class is not used in the TinyXML data model.
    
    \filename SerializationTest.h
*/
class SerializationTest {
public:

    TICPP_READWRITE

    enum test_t {
        t_x1,                                               // Keyword: X1
        t_x2,                                               // Keyword: X2
        NUM_test
    };

    enum intPara_t {
        IP_i1,                                              // Keyword: I1
        IP_i2,                                              // Keyword: I2
        NUM_IP
    };

    enum splinePara_t {
        SP_ParameterSet1,                                   // Keyword: ParameterSet1
        SP_ParameterSet2,                                   // Keyword: ParameterSet2
        NUM_SP
    };


    enum ReferencedIDTypes {
        SomeStove,                                          // Keyword: SomeStove
        SomeOven,                                           // Keyword: SomeOven
        SomeHeater,                                         // Keyword: SomeHeater
        SomeFurnace,                                        // Keyword: SomeFurnace
        NUM_RefID
    };

    // -> id1="5"
    int                 m_id1       = 5;                    // XML:A:required
    // -> id2="10"
    unsigned int        m_id2       = 10;                   // XML:A
    // -> flag1="0"
    bool                m_flag1     = false;                // XML:A
    // -> val1="42.42"
    double              m_val1      = 42.42;                // XML:A
    // -> testBla="X1"
    test_t              m_testBla   = t_x1;                 // XML:A
    // -> str1="Blubb"
    std::string         m_str1      = "Blubb";              // XML:A
    // -> path1="/tmp"
    IBK::Path           m_path1     = IBK::Path("/tmp");    // XML:A
    // -> u1="K"
    IBK::Unit           m_u1        = IBK::Unit("K");       // XML:A

    // -> <Id3>10</Id3>
    int                 m_id3       = 10;                   // XML:E:required
    // -> <Id4>12</Id4>
    unsigned int        m_id4       = 12;                   // XML:E
    // -> <Flag2>1</Flag2>
    bool                m_flag2     = true;                 // XML:E
    // -> <Val2>41.41</Val2>
    double              m_val2      = 41.41;                // XML:E
    // -> <TestBlo>X2</TestBlo>
    test_t              m_testBlo   = t_x2;                 // XML:E
    // -> <Str2>blabb</Str2>
    std::string         m_str2      = "blabb";              // XML:E

    // -> <Path2>/var</Path2>
    IBK::Path           m_path2     = IBK::Path("/var");    // XML:E
    // -> undefined/empty - not written
    IBK::Path           m_path22;                           // XML:E

    // -> <U2>C</U2>
    IBK::Unit           m_u2        = IBK::Unit("C");       // XML:E
    // -> <X5>43.43</X5>
    double              m_x5        = 43.43;                // XML:E

    // -> <IBK:Flag name="F">true</IBK:Flag>  -> value of m_f.name is ignored
    IBK::Flag           m_f;                                // XML:E
    // -> undefined/empty - not written
    IBK::Flag           m_f2;                               // XML:E

    // -> <Time1>01.01.07 12:47:12</Time1>
    IBK::Time           m_time1;                            // XML:E
    // -> undefined/empty - not written
    IBK::Time           m_time2;                            // XML:E

    // -> <Table>Col1:1,5,3;Col2:7,2,2;</Table>
    DataTable           m_table;                            // XML:E
    // -> undefined/empty - not written
    DataTable           m_table2;                           // XML:E

    // ->       <DblVec>0,12,24</DblVec>
    std::vector<double>     m_dblVec;                       // XML:E

    // -> <Interfaces>...</Interfaces>
    std::vector<Interface>  m_interfaces;                   // XML:E

    // -> <InterfaceA>....</InterfaceA>  instead of <Interface>..</Interface>
    Interface               m_interfaceA;                   // XML:E:tag=InterfaceA

    // -> <IBK:Parameter name="SinglePara" unit="C">20</IBK:Parameter>
    IBK::Parameter      m_singlePara;                       // XML:E

    // -> <IBK:IntPara name="SingleIntegerPara">12</IBK:IntPara>
    IBK::IntPara        m_singleIntegerPara = IBK::IntPara("blubb",12); // XML:E

    // -> <IBK:Parameter name="X1" unit="C">12</IBK:Parameter>
    IBK::Parameter      m_para[NUM_test];                       // XML:E

    // -> <IBK:IntPara name="I1">13</IBK:IntPara>
    IBK::IntPara        m_intPara[NUM_IP];                      // XML:E

    // -> <IBK:Flag name="X2">true</IBK:Flag>
    IBK::Flag           m_flags[NUM_test];                      // XML:E

    IDType              m_someStuffIDAsAttrib;                  // XML:A
    IDType              m_someStuffIDAsElement;                 // XML:E

    // -> <SomeStove>231</SomeStove> : Keywords must be unique!
    IDType              m_idReferences[NUM_RefID];              // XML:E

    // -> <IBK:LinearSpline name="LinSpl">...</IBK:LinearSpline>
    IBK::LinearSpline   m_linSpl;                               // XML:E

    // -> <LinearSplineParameter name="AnotherSplineParameter">...</LinearSplineParameter>
    LinearSplineParameter           m_anotherSplineParameter;   // XML:E

    // generic class with own readXML() and writeXML() function
    // -> <Schedule...>...</Schedule>
    Schedule            m_sched;                                // XML:E

    // generic class with custom tag name
    // -> <OtherSchedule...>...</OtherSchedule>
    Schedule            m_sched2;                               // XML:E:tag=OtherSchedule

    // -> <Point2D>x,y</Point2D>
    IBK::point2D<double>    m_coordinate2D;                     // XML:E
};
```

Mind the macro `TICPP_READWRITE`.

### Generate code

```bash
# SYNTAX: TinyXMLCodeGenerator <namespace> <path/to/src> <generateQtSrc> <prefix> <cg-dir>
> TinyXMLCodeGenerator MyNameSpace /path/to/MyProject/src 0 MyNameSpace cg
```

This will generate the file `/path/to/MyProject/src/cg/cg_SerializationTest.h` which contains the implementations of the functions `read()` and `write()`.


