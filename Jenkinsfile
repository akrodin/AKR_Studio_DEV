node {  
   stage('Checkout') {
     echo "Preparation Stage"     
     checkout scm 
   }    
   stage('Publish Script Package') {
     sh '''
        echo 'Iterating over all XML files in repo'
        search_dir=$(find . -name '*.XML')
        
        BASE_DIRECTORY=$(dirname ${search_dir})
        echo "#$BASE_DIRECTORY#";
        cd $BASE_DIRECTORY
            
        for i in $search_dir;
        do
            echo $i
            echo 'Convert file to base64 and invoke CXone API to push it'

            base_name=$(basename ${i})
            base64 $base_name > encodedData.txt
            encoded="$(cat encodedData.txt)"
            
            file_name_without_extension=${base_name%.*}        
            script_path='AKR_CI_CD\\\\'"${file_name_without_extension}"                        
            
            payload='{"scriptPath": "'$script_path'","body":"'$encoded'"}'
                       
            token='eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ1c2VyOjExZTljYjUzLWEyZjktNDUwMC1hNDA3LTAyNDJhYzExMDAwMyIsInJvbGUiOnsibGVnYWN5SWQiOiJBZG1pbmlzdHJhdG9yLTE0MmM0OTU5LTVmYjYtNGFkNC05NWJkLWM1ODQ0YzExMDg0MyIsInNlY29uZGFyeVJvbGVzIjpbXSwiaWQiOiIxMWVjYzU3OS1lMGJkLWMyZDAtYmU3Zi0wMjQyYWMxMTAwMDMiLCJsYXN0VXBkYXRlVGltZSI6MTY5NDcwMDk3NjAwMH0sImljQWdlbnRJZCI6IjcxNDEyMDkiLCJpc3MiOiJodHRwczpcL1wvYXV0aC5uaWNlLWluY29udGFjdC5jb20iLCJnaXZlbl9uYW1lIjoiQWxsb24iLCJhdWQiOiJpbmNvbnRhY3QgYWRtaW5AaW5jb250YWN0IGluYy4iLCJpY1NQSWQiOiIyNDMyIiwiaWNCVUlkIjo0NTk3MzU5LCJuYW1lIjoiYXJvZGluQGIzMi5jb20iLCJ0ZW5hbnRJZCI6IjExZTg0NzNhLTdmYTYtZTc0MC04NzgzLTAyNDJhYzExMDAwNiIsImV4cCI6MTY5NzgxMzUzMywiaWF0IjoxNjk3ODA5OTMzLCJmYW1pbHlfbmFtZSI6IlJvZGluIiwidGVuYW50IjoiU0VfRGVtb19CMzIiLCJ2aWV3cyI6e30sImljQ2x1c3RlcklkIjoiQjMyIn0.MNOCObpC1Z2bUcugBtRiZZo_ErLNRQT52i4zKWJIg0-4emLyQzX9xPs5vGEdymhYN-XMwXUdzcX-wxBivvwExjsCJxbaYi-RbI1elSpWiJfbfTrgYVL72tNTBP0_IDH-F5PRJjR0OXEfejNESCpfv63jNWskFsam6ijonjjRXOc'

            result=$(curl --location --request POST \
                'https://api-b32.nice-incontact.com/inContactAPI/services/v18.0/scripts' --header 'accept: */*' \
                --header 'Content-Type: application/json' \
                --header 'Authorization: Bearer '"$token" \
                --data-raw "$payload") 
            echo "Result is $result"

        done;
      '''
   }   
}
