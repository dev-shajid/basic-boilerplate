# Basic Setup of Formik

**1.Installation**
```
npm install formik
```
---

**2.Formik Hook Call**
```javascript
import React, { useState } from 'react'
import { useFormik } from 'formik';
import { validateRegForm } from '../helper/validate'

export default function Form(){
    const [errors, setErrors]=useState({})

    const formik = useFormik({
        initialValues: {fullName: '', email: '', password: ''},
        validate: async (values) => {
        setErrors(await validateRegForm(values))
        },
        validateOnBlur: true,
        validateOnChange: false,
        onSubmit: async (values) => {
            if (!Object.keys(errors).length) {
                // form submission function
            }
        }
    })

    return(
        <>
            <form onSubmit={formik.handleSubmit}>
                <input
                    {...formik.getFieldProps('fullName')}
                />
                <input
                    {...formik.getFieldProps('email')}
                />
                <input
                    {...formik.getFieldProps('password')}
                />
            </form>
        </>
    )
}
```
---
**3.Validation Function**

`In helper folder, a file validate.js `
```javascript
export async function validateRegForm(values) {
    const errors = {}
    fullNameVerify(errors, values)
    emailVerify(errors, values)
    passwordVerify(errors, values)
    phoneVerify(errors, values)

    return errors
}

export async function validateLoginForm(values) {
    const errors = {}
    emailVerify(errors, values)
    passwordVerify(errors, values)

    return errors
}

/** ************************ Verify Function ************************* */

function fullNameVerify(errors, values) {
    if (!values.fullName?.trim()) {
        errors.fullName = 'Fullname is Required...!'
    }

    return errors;
}

function emailVerify(errors, values) {
    if (!values.email?.trim()) {
        errors.email = 'Email is Required...!'
    }
    else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email) || values.email.trim().includes(" ")) {
        errors.email = "Invalid Email address...!"
    }


    return errors;
}

function passwordVerify(errors, values) {
    if (!values.password?.trim()) {
        errors.password = 'Password is Required...!'
    }
    else if (values.password.includes(" ")) {
        errors.password = "Password should not containe space"
    }
    else if (values.password.length < 6) {
        errors.password = "Password length must be atleast 6 "
    }

    return errors;
}

function phoneVerify(errors, values) {
    let pattern = "(?:\\+88|88)?(01[3-9]\\d{8}$)"
    if (!values.phone?.trim()) {
        errors.phone = 'Phone Number is Required...!'
    }
    else if (!values.phone.match(pattern)) {
        errors.phone = "Invalid Phone Number"
    }

    return errors;
}

```
-----
### Conver image to Base64 Function
In helper folder/ convert.js
```javascript
export default function convertToBase64(file){
    return new Promise((resolve, reject) => {
        const fileReader = new FileReader();
        fileReader.readAsDataURL(file);

        fileReader.onload = () => {
            resolve(fileReader.result)
        }

        fileReader.onerror = (error) => {
            reject(error)
        }
    })
}
```

---

**Handle Image in Formik**
```javascript
import React, { useState } from 'react'
import { useFormik } from 'formik';
import { validateRegForm } from '../helper/validate'

export default function Form(){
    const [errors, setErrors]=useState({})

    const formik = useFormik({
        initialValues: {fullName: '', email: '', password: ''},
        validate: async (values) => {
        setErrors(await validateRegForm(values))
        },
        validateOnBlur: true,
        validateOnChange: false,
        onSubmit: async (values) => {
            if (!Object.keys(errors).length) {
                // form submission function
            }
        }
    })

    const handleProfile = async (e) => {
        const base64 = await convertToBase64(e.target.files[0]);
        formik.setFieldValue('profile', base64);
        console.log(formik.values.profile);
    }

    return(
        <>
            <input 
                onChange={handleProfile} 
                name='student_profile' 
                className='hidden' id='student_profile' type="file"     
            />
            <Button variant='outlined' sx={{ padding: '0' }}>
              <label className='cursor-pointer' htmlFor="student_profile">
                {
                  formik.values.profile
                    ? <img 
                        className='w-[150px] aspect-square object-cover' 
                        src={formik.values.profile} alt={formik.values.fname} 
                    />
                    : <div className='flex items-center justify-center px-6 py-3'>

                        <span className='ml-2 leading-6'>Student Profile</span>
                    </div>
                }
              </label>
            </Button>
        </>
    )
}
```
---
**Set Value Manually in Formik**
```javascript
    formik.setFieldValue('fullName', value)
```
---
**Access Value Manually in Formik**
```javascript
    formik.values.fullName
```
---
**Set all value after fetching Formik and useEffect**
```javascript
    useEffect(() => {
        async function setInitialValues() {
            if (GetCourse.data) await formik.setValues(GetCourse.data)
        }
        setInitialValues();
    }, [GetCourse.data])
```