## C Generic JSON Serializer/Deserializer
 
Project is based on JSMN tokenizer.
https://github.com/zserge/jsmn

## Code Example

Structure definition:

	#define FOREACH_TEST_PARAMETER(PARAMETER, CLASSTYPE)\
          PARAMETER(VarName1,   SER_TYPE_FLOAT,  0, CLASSTYPE) \ 
          PARAMETER(A2,         SER_TYPE_STRING, 0, CLASSTYPE) \
          PARAMETER(A3,         SER_TYPE_INT,    0, CLASSTYPE)
		 
	typedef struct{
	  FOREACH_TEST_PARAMETER(GENERATE_SER_GENERIC,)
	}TestStruct_t;

Serialization information:

	const SerializationInfo_t TEST_SERIALIZATION_INFO = {
	    SERIALIZATION_START("Test","TestNamespace", TestStruct_t),
	    FOREACH_TEST_PARAMETER(GENERATE_SERIALIZABLE, TestStruct_t)
	    SERIALIZATION_END()
	};
	
Serializtion information could be also created manualy.
Every member that is put into serializerion info is private member.

Function example:

	#define BUF_SIZE 1024
	#define JSON_MAX_TOKENS 50

	void TestSerDeser(){
		 char buf[BUF_SIZE];
		 int len;
		 int tcnt;

		 TestStruct_t obj;
		 len = JsonSerialize(obj, TEST_SERIALIZATION_INFO, buf, BUF_SIZE);

		 if(len<0)return;//buffer to small

		 //deserialize test
		 jsmntok_t tokens[JSON_MAX_TOKENS];
		 jsmn_parser jsmnparser;
		 jsmn_init(&jsmnparser);
		 tcnt = jsmn_parse(&context->jsonparser, buf, len, tokens, JSON_MAX_TOKENS);
		 if(tcnt<=0) return ;//error

		 JsonDeserialize(buf, tokens, tcnt, obj, TEST_SERIALIZATION_INFO);	
	}




## License

This software is distributed under MIT license, so feel free to integrate it in your commercial products.
