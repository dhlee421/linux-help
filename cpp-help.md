```cpp

void logmessage (const char *str,...)
{
  va_list arg_ptr;
  va_start(arg_ptr, str);
  
  char buffer[1024]="";
  
  _vsprintf_p(buffer, 1024, str, arg_ptr);
  
  OutputDebugString(buffer);
}
