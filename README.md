## Review Feedback

we are using the SignUp Function given on Sample code.
```javascript
const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
  event.preventDefault();

  try {
    const response = await fetch(`${API_ENDPOINT}/organisations`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ name: organisationName, user_name: userName, email: userEmail, password: userPassword }),
    });

    if (!response.ok) {
      throw new Error(`Sign-up failed with status ${response.status}`);
    }
    console.log('Sign-up successful');

    const data = await response.json();
    localStorage.setItem('authToken', data.token);
    localStorage.setItem('userData', JSON.stringify(data.user));
    navigate("/dashboard");
  } catch (error) {
    console.error('Sign-up failed:', error);
  }
};
```

The Updated one 
```javascript

/**
 * 
 * this function handles the form of signup
 * @param {React.FormEvent<HTMLFormElement} event - the form event an React Element
 */
const handleSubmitSignUp = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
  
    try {
      const response = await fetch(`${API_ENDPOINT}/organisations`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ name: organisationName, user_name: userName, email: userEmail, password: userPassword }),
      });
  
      if (!response.ok) {
        throw new Error(`Sign-up failed with status ${response.status}`);
      }
        console.log('Sign-up successful');
        /*
        * Exception handling is done if json failed.
        */
        try {
            const data = await response.json();
        }
        catch (err) {
            throw new Error('Invalid Json response ');
        }
        /* Set the auth  token and User Data in local storage and redirect to dashboard page*/
      localStorage.setItem('authToken', data.token);
      localStorage.setItem('userData', JSON.stringify(data.user)); 
      navigate("/dashboard");
    } catch (error) {
        console.error('Sign-up failed:', error);// Show reason for fail in console
    }
  };
  ```


## Iterative Development Proces

![image](https://github.com/KartikAgarwal977/wd401/assets/101928227/b06cc8ae-da95-41fd-9f0a-3c5359dd50ba)

1. Start by understanding the feedback received from code reviews.Make necessary changes or improvements based on the feedback.
2. Test the updated code to ensure it functions correctly.
3. Submit the changes for another round of code review.
4. Repeat the process until the feature meets all requirements and quality standards.

### Resolving Merge Conflict
In a hypothetical scenario where merge conflicts arise, follow these steps to resolve them within a pull-request:
1. Identify the conflicting files by reviewing the merge conflict message.
2. Open the conflicting files in your code editor.
3. Manually resolve the conflicts by editing the conflicting sections.
4. Save the changes and stage the resolved files.
5. Commit the changes with a descriptive message explaining the resolution.
6. Push the changes to the feature branch.
7. Close the pull-request and request a re-review if necessary.

### CI/CD Integration:

1. Include automated tests for the feature to validate its functionality.
2. Set up linting and code formatting checks to maintain code quality.

``` javascript
test('handleSubmit submits form data successfully', async () => {
  // Mock fetch function
  global.fetch = jest.fn().mockResolvedValue({ ok: true, json: () => ({ token: 'mockToken', user: { id: 1, name: 'John' } }) });

  // Mock form data
  const mockEvent = { preventDefault: jest.fn() };
  const mockFormData = {
    organisationName: 'TestOrg',
    userName: 'testUser',
    userEmail: 'test@example.com',
    userPassword: 'password123',
  };

  await handleSubmit(mockEvent, mockFormData);

  expect(global.fetch).toHaveBeenCalledWith(`${API_ENDPOINT}/organisations`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(mockFormData),
  });
  expect(localStorage.getItem('authToken')).toEqual('mockToken');
  expect(localStorage.getItem('userData')).toEqual(JSON.stringify({ id: 1, name: 'John' }));
});
   ```
