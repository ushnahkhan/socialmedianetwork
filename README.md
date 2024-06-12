#include <iostream>
#include <fstream>
using namespace std;
class Page;
class User;

class Date
{
	int day;
	int month;
	int year;
	static Date CurrentDate;
public:
	
	Date(int d = 0, int m = 0, int y = 0)
	{
		day = d;
		month = m;
		year = y;
	}
	static void setCurrentDate(int d, int m, int y)
	{
		cout << "--------------------------------------------------------------------\n";
		cout << "Set Current System Date " <<d << " " << m << " " << y << endl;
		CurrentDate.day = d;
		CurrentDate.month = m;
		CurrentDate.year = y;
	}
	static Date getCurrentDate()
	{
		return CurrentDate;
	}
	static void PrintCurrentDate()
	{
		cout << CurrentDate.day << "/" << CurrentDate.month << "/" << CurrentDate.year<<endl;
	}
	void PrintDate()
	{
		if (isPostabledifference())
		{
			if (day == CurrentDate.day && month == CurrentDate.month && year == CurrentDate.year)
			{
				cout << "(1h):\n";
			}
			else if (month == CurrentDate.month && year == CurrentDate.year)
			{
				if (day < CurrentDate.day)
				{
					cout << "(" << (CurrentDate.day-day) << "d):\n";
				}
			}
			else if (year == CurrentDate.year)
			{
				if (month <CurrentDate.month)
				{
					cout << "(" << (CurrentDate.month - month) << "m):\n";
				}
			}
			else
			{
				cout << "(" << day << "/" << month << "/" << year << "):\n";
			}
		}

	}
	bool isPostabledifference()
	{
		if (year == CurrentDate.year && month == CurrentDate.month)
		{
			if (day > CurrentDate.day)
			{
				return false;
			}
		}
		else if (year == CurrentDate.year)
		{
			if (month > CurrentDate.month)
			{
				return false;
			}
		}
		else if (year > CurrentDate.year)
		{
			return false;
		}
		return true;
	}
	bool isDifference()
	{
		if (year == CurrentDate.year && month == CurrentDate.month)
		{
			if (day <=CurrentDate.day)
			{
				if (CurrentDate.day-day< 2)
				{
					return true;
				}
			}
		}
		return false;
	}
	int CalculateYearDifference()
	{
		if (CurrentDate.month == month && CurrentDate.day == day)
		{
			return (CurrentDate.year - year);
		}
		else
			return 0;
	}
	
};

Date Date::CurrentDate(17, 4, 2024);

class Post;
class Object;
class Controller;
class Comment;

class PostContent
{
public:
	virtual void Print()
	{

	}
	virtual void ReadFromFile(ifstream& fin)
	{

	}
	virtual ~PostContent()
	{

	}
};
class Activity :public PostContent
{
	int type;
	char* value;
	char* GetStringFromBuffer(char* t)
	{
		int length = strlen(t);
		char* returnstr = new char[length + 1];
		for (int i = 0; i < length; i++)
		{
			returnstr[i] = t[i];
		}
		returnstr[length] = 0;
		return returnstr;
	}
public:
	Activity(int t=0)
	{
		type = t;
		value = 0;
	}
	void Print()
	{
		if (type == 1)
		{
			cout << " is feeling " << value<<" ";
		}
		else if (type == 2)
		{
			cout << " is thinking about " << value<<" ";
		}
		else if (type == 3)
		{
			cout << " is making " << value<<" ";
		}
		else if (type == 4)
		{
			cout << " is celebrating " << value<<" ";
		}
		else if (type == 5)
		{
			cout << " has shared a memory ";
		}
	}
	void ReadFromFile(ifstream&fin)
	{
		fin.ignore();
		fin >> type;
		char temp[50];
		fin.ignore();
		fin.getline(temp, 50);
		value = GetStringFromBuffer(temp);
	}
	~Activity()
	{
		if (value)
			delete[]value;
	}
};

class Post
{
	char* ID;
	char* text;
	int nooflikes;
	Date sharedDate;
	Object* SharedBy;
	Object** LikedBy;
	Comment** comments;
	PostContent* content;
	static int noofposts;
	bool StringCompare(char* d, char s[])
	{
		if (strlen(d) == strlen(s))
		{
			int flag = 0;
			for (int i = 0;i < strlen(d) && flag == 0;i++)
			{
				if (d[i] != s[i])
				{
					flag = 1;
				}
			}
			if (flag == 1)
				return false;
			else
				return true;
		}
		else
			return false;
	}
	char* stringConcatenate(const char* cstring1, int num)
	{
		int temp = num;
		int n = 0;
		while (temp != 0)
		{
			n++;
			temp /= 10;
		}
		char* cstring2 = new char[n + 1];
		for (int i = n - 1;i >= 0;i--)
		{
			cstring2[i] = (num % 10) + 48;
			num /= 10;
		}
		cstring2[n] = 0;
		int length = strlen(cstring1) + strlen(cstring2);
		char* newstring = new char[length + 1];
		int i = 0;

		for (int j = 0; j < strlen(cstring1); j++, i++)
		{
			newstring[i] = cstring1[j];
		}

		for (int j = 0; j < strlen(cstring2); j++, i++)
		{
			newstring[i] = cstring2[j];
		}
		newstring[i] = '\0';

		return newstring;
	}
	char* GetStringFromBuffer(char t[])
	{
		int length = strlen(t);
		char* returnstr = new char[length + 1];
		for (int i = 0; i < length; i++)
		{
			returnstr[i] = t[i];
		}
		returnstr[length] = 0;
		return returnstr;
	}
public:
	Post()
	{
		ID = 0;
		text = 0;
		nooflikes = 0;
		SharedBy = 0;
		LikedBy = 0;
		comments = 0;
		content = 0;
		noofposts++;
	}
	Post(char* t, Object*& o, Date d)
	{
		noofposts++;
		ID = stringConcatenate("post", noofposts);
		SharedBy = o;
		text = t;
		sharedDate = d;
		nooflikes = 0;
		LikedBy = 0;
		comments = 0;
		content = 0;

	}
	void PrintID()
	{
		cout << ID;
	}
	bool isSameId(char* id)
	{
		if (StringCompare(id, ID))
		{
			return true;
		}
		return false;
	}
	void SetSharedBy(Object*& o)
	{
		SharedBy = o;
	}
	void PrintLikes();
	void ReadDataFromFile(ifstream& fin)
	{
		char temp[100];
		fin >> temp;
		ID = GetStringFromBuffer(temp);
		int day, mon, year;
		fin >> day;
		fin >> mon;
		fin >> year;
		Date d(day, mon, year);
		sharedDate = d;
		fin.ignore();
		fin.getline(temp, 100);
		text = GetStringFromBuffer(temp);
	}
	bool issufficentdifference()
	{
		bool flag;
		flag = sharedDate.isDifference();
		return flag;
	}
	int yeardifference()
	{
		return sharedDate.CalculateYearDifference();
	}
	bool isPostable()
	{
		return sharedDate.isPostabledifference();
	}
	bool isSharedBy(Object* o)
	{
		if (SharedBy == o)
		{
			return true;
		}
		else
		{
			return false;
		}
	}
	void SetLikedBy(Object*& o)
	{
		if (!LikedBy)
		{
			LikedBy = new Object * [11] {0};
		}
		int flag = 0;
		int i = 0;
		while (LikedBy[i] != 0 && flag == 0)
		{
			if (LikedBy[i] == o)
				flag = 1;
			i++;
		}
		if (flag == 0)
		{
			LikedBy[nooflikes] = o;
			nooflikes++;
		}
	}
	void AddComment(Comment*& c);
	void AddContent(PostContent*& pc)
	{
		content = pc;
	}
	virtual void PrintListView();
	
	virtual ~Post();
	
};
int Post::noofposts = 0;
class Object
{
private:
	char* ID;
	bool StringCompare(char* d, char s[])
	{
		if (strlen(d) == strlen(s))
		{
			int flag = 0;
			for (int i = 0;i < strlen(d) && flag == 0;i++)
			{
				if (d[i] != s[i])
				{
					flag = 1;
				}
			}
			if (flag == 1)
				return false;
			else
				return true;
		}
		else
			return false;
	}
	char* GetStringFromBuffer(char* t)
	{
		int length = strlen(t);
		char* returnstr = new char[length + 1];
		for (int i = 0; i < length; i++)
		{
			returnstr[i] = t[i];
		}
		returnstr[length] = 0;
		return returnstr;
	}
protected:
	Post** Timeline;
public:
	Object()
	{
		ID = 0;
		Timeline = 0;
	}
	void SetID(char* id)
	{
		ID = GetStringFromBuffer(id);
	}
	virtual void PrintID()
	{
		cout << ID;
	}
	virtual void PrintName()
	{

	}
	virtual void PrintListView()
	{

	}
	virtual bool isSameID(char* id)
	{
		if (StringCompare(ID, id))
		{
			return true;
		}
		return false;
	}
	void AddToTimeline(Post*& p)
	{
		if (!Timeline)
		{
			Timeline = new Post * [11] {0};
		}
		int i = 0;
		while (i < 10 && Timeline[i] != 0)
		{
			i++;
		}
		if (i < 10)
		{
			for (int j = i;j > 0;j--)
			{
				Timeline[j] = Timeline[j - 1];
			}
			Timeline[0] = p;
		}
	}
	void ViewTimeline();
	
	virtual ~Object()
	{
		if (ID)
			delete[]ID;
		if (Timeline)
		{
			for (int i = 0;Timeline[i] != 0;i++)
			{
				delete Timeline[i];
			}
			delete[]Timeline;
		}
	}
};
void Post::PrintLikes()
{
	int i = 0;
	if (LikedBy)
	{
		while (LikedBy[i] != 0)
		{
			LikedBy[i]->PrintListView();
			i++;
		}
	}
	else
	{
		cout << "LIKED BY NONE" << endl;
	}
}

class Comment
{
	char* ID;
	Object* commentBy;
	char* text;
	static int noofcomments;
	char* stringConcatenate(const char* cstring1, int num)
	{
		int temp = num;
		int n = 0;
		while (temp != 0)
		{
			n++;
			temp /= 10;
		}
		char* cstring2 = new char[n + 1];
		for (int i = n - 1;i >= 0;i--)
		{
			cstring2[i] = (num % 10) + 48;
			num /= 10;
		}
		cstring2[n] = 0;
		int length = strlen(cstring1) + strlen(cstring2);
		char* newstring = new char[length + 1];
		int i = 0;

		for (int j = 0; j < strlen(cstring1); j++, i++)
		{
			newstring[i] = cstring1[j];
		}

		for (int j = 0; j < strlen(cstring2); j++, i++)
		{
			newstring[i] = cstring2[j];
		}
		newstring[i] = '\0';

		return newstring;
	}
public:
	Comment(char*id,Object*&o,char*t)
	{
		ID = id;
		commentBy = o;
		text = t;
		noofcomments++;
	}
	Comment(Object*& o, char* t)
	{
		noofcomments++;
		ID = stringConcatenate("c", noofcomments);
		commentBy = o;
		text = t;
	}
	void PrintComment()
	{
		commentBy->PrintName();
		cout << ": " << text << endl;
	}
	~Comment()
	{
		if (ID)
			delete[]ID;
		if (text)
			delete[]text;
		if (commentBy)
			commentBy = 0;
	}
};
int Comment::noofcomments = 0;
Post::~Post()
{
	if (ID)
		delete[]ID;
	if (text)
		delete[]text;
	if (SharedBy)
		SharedBy = 0;
	if (LikedBy)
	{
		for (int i = 0;LikedBy[i] != 0;i++)
			LikedBy[i]=0;
		delete[] LikedBy;
	}
	if (comments)
	{
		for (int i = 0;comments[i] != 0;i++)
			delete comments[i];
		delete[] comments;
	}
	if (content)
		delete content;

}
void Post::PrintListView()
{
	SharedBy->PrintName();
	if (content)
	{
		content->Print();
	}
	sharedDate.PrintDate();
	char doublequotes = 34;
	cout << "\t" << doublequotes << text << doublequotes << endl;
	int i = 0;
	if (comments)
	{
		while (comments[i] != 0)
		{
			cout << "\t\t";
			comments[i]->PrintComment();
			i++;
		}
	}
}
void Post::AddComment(Comment*& c)
	{
		if (!comments)
		{
			comments = new Comment * [11] { 0 };
		}
		int i = 0;
		while (i < 10 && comments[i] != 0)
		{
			i++;
		}
		if (i <= 10)
		{
			comments[i] = c;
		}
	}
class Page :public Object
{
	char* Title;
	char* GetStringFromBuffer(char t[])
	{
		int length = strlen(t);
		char* returnstr = new char[length + 1];
		for (int i = 0; i < length; i++)
		{
			returnstr[i] = t[i];
		}
		returnstr[length] = 0;
		return returnstr;
	}
public:
	Page()
	{
		Title = 0;
		Timeline = 0;
	}
	void ReadDataFromFile(ifstream& fin)
	{
		char temp[50];
		fin >> temp;
		Object::SetID(temp);
		fin.ignore();
		fin.getline(temp, 50);
		Title = GetStringFromBuffer(temp);
	}
	void PrintName()
	{
		cout << Title<<" ";
	}
	void PrintListView()
	{
		Object::PrintID();
		cout << "-" << Title << endl;
	}
	void PrintLatest()
	{
		if (Timeline)
		{
			for (int i = 0;i < 10;i++)
			{
				if (Timeline[i] != 0)
				{
					if (Timeline[i]->issufficentdifference())
					{
						Timeline[i]->PrintListView();
					}
				}
			}
		}
	}
	~Page()
	{
		if (Title)
			delete[]Title;
	}
};

class User :public Object
{
	char* Fname;
	char* Lname;
	User** FriendsList;
	Page** LikedPages;
	char* GetStringFromBuffer(char t[])
	{
		int length = strlen(t);
		char* returnstr = new char[length + 1];
		for (int i = 0; i < length; i++)
		{
			returnstr[i] = t[i];
		}
		returnstr[length] = 0;
		return returnstr;
	}

public:
	User()
	{
		Fname = 0;
		Lname = 0;
		FriendsList = 0;
		LikedPages = 0;
		Timeline = 0;
	}
	void PrintName()
	{
		cout << Fname << " " << Lname << " ";
	}
	void ReadDataFromFile(ifstream& fin)
	{
		char temp[50];
		fin >> temp;
		Object::SetID(temp);
		fin >> temp;
		Fname = GetStringFromBuffer(temp);
		fin >> temp;
		Lname = GetStringFromBuffer(temp);

	}
	void AddFriend(User*& u)
	{
		if (!FriendsList)
		{
			FriendsList = new User * [11] {0};
		}
		int flag = 0;
		for (int i = 0;i < 10 && flag == 0;i++)
		{
			if (FriendsList[i] == u)
			{
				flag = 1;
			}
		}
		if (flag == 0)
		{
			for (int i = 0;i < 10 && flag == 0;i++)
			{
				if (FriendsList[i] == 0)
				{
					FriendsList[i] = u;
					flag = 1;
				}
			}
		}
	}
	void LikePage(Page*& p)
	{
		if (!LikedPages)
		{
			LikedPages = new Page * [11] {0};
		}
		int flag = 0;
		for (int i = 0;i < 10 && flag == 0;i++)
		{
			if (LikedPages[i] == p)
			{
				flag = 1;
			}
		}
		if (flag == 0)
		{
			for (int i = 0;i < 10 && flag == 0;i++)
			{
				if (LikedPages[i] == 0)
				{
					LikedPages[i] = p;
					flag = 1;
				}
			}
		}
	}
	void PrintListView()
	{
		Object::PrintID();
		cout<< "-" << Fname << " " << Lname << endl;
	}
	void ViewFriendsList()
	{
		
		cout << Fname << " " << Lname << " - Friends List:" << endl;
		if (FriendsList)
		{
			for (int i = 0;i < 10;i++)
			{
				if (FriendsList[i] != 0)
				{
					FriendsList[i]->PrintListView();
				}
			}
		}
		else
		{
			cout << "THIS USER HAS NO FRIENDS" << endl;
		}
	}
	void ViewLikedPages()
	{
		cout << Fname << " " << Lname << " - Liked Pages:" << endl;
		if (LikedPages)
		{
			for (int i = 0;i < 10;i++)
			{
				if (LikedPages[i] != 0)
				{
					LikedPages[i]->PrintListView();
				}
			}
		}
		else
		{
			cout << "THIS USER HAS NO LIKED PAGES" << endl;
		}
	}
	void PrintLatest()
	{
		if (Timeline)
		{
			for (int i = 0;i < 10;i++)
			{
				if (Timeline[i] != 0)
				{
					if (Timeline[i]->issufficentdifference())
					{
						Timeline[i]->PrintListView();
					}
				}
			}
		}
	}
	void ViewHome()
	{
		cout << Fname << " " << Lname << " - Home Page\n\n";
		PrintLatest();
		if (FriendsList)
		{
			for (int i = 0;FriendsList[i] != 0;i++)
			{
				if (FriendsList[i])
				{
					FriendsList[i]->PrintLatest();
				}
			}
		}
		if (LikedPages)
		{
			for (int i = 0;LikedPages[i] != 0;i++)
			{
				if (LikedPages[i])
				{
					LikedPages[i]->PrintLatest();
				}
			}
		}
		cout << "END OF HOME PAGE" << endl << endl;
	}
	~User()
	{
		if (Fname)
			delete[]Fname;
		if (Lname)
			delete[]Lname;
		int i = 0;
		if (FriendsList)
		{
			while (FriendsList[i] != 0)
			{
				FriendsList[i] = 0;
				i++;
			}
			delete[]FriendsList;
		}
		i = 0;
		if (LikedPages)
		{
			while (LikedPages[i] != 0)
			{
				LikedPages[i] = 0;
				i++;
			}
			delete[]LikedPages;
		}
	}
};

class Memory :public Post
{
	Post* originalPost;
	
	char* GetStringFromBuffer(const char* t)
	{
		int length = strlen(t);
		char* returnstr = new char[length + 1];
		for (int i = 0; i < length; i++)
		{
			returnstr[i] = t[i];
		}
		returnstr[length] = 0;
		return returnstr;
	}
public:
	Memory(const char* t, Post* p, Object* o):Post(GetStringFromBuffer(t),o, Date::getCurrentDate())
	{
		originalPost = p;
		PostContent* pc = new Activity(5);
		AddContent(pc);
	}
	void PrintListView()
	{
		Post::PrintListView();
		cout << "\t\t" << originalPost->yeardifference() << " years ago\n";
		cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
		cout << "\t\t";
		originalPost->PrintListView();
		cout << "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~" << endl;
	}
	~Memory()
	{
		originalPost = 0;
	}
};
void Object::ViewTimeline()
{
	PrintName();
	cout << " - Timeline" << endl;
	int flag = 0;
	if (Timeline)
	{
		for (int i = 0;i < 10;i++)
		{
			if (Timeline[i] != 0)
			{
				if (Timeline[i])
				{
					if (Timeline[i]->isPostable())
					{
						Timeline[i]->PrintListView();
						flag = 1;
					}
				}
			}
		}
	}
	if (flag == 0)
	{
		cout << endl << "NOTHING ON TIMELINE" << endl;
	}
}


class Controller
{
	User** AllUsers;
	Page** AllPages;
	Post** AllPosts;
	bool StringCompare(char* d, char s[])
	{
		if (strlen(d) == strlen(s))
		{
			int flag = 0;
			for (int i = 0;i < strlen(d) && flag == 0;i++)
			{
				if (d[i] != s[i])
				{
					flag = 1;
				}
			}
			if (flag == 1)
				return false;
			else
				return true;
		}
		else
			return false;
	}
	char* GetStringFromBuffer(const char t[])
	{
		int length = strlen(t);
		char* returnstr = new char[length + 1];
		for (int i = 0; i < length; i++)
		{
			returnstr[i] = t[i];
		}
		returnstr[length] = 0;
		return returnstr;
	}
	
public:
	Controller()
	{
		AllPages = 0;
		AllPosts = 0;
		AllUsers = 0;
	}
	User* SearchUserByID(char t[])
	{
		int flag = 0;
		int i = 0;
		while (AllUsers[i] != 0 && flag == 0)
		{
			if (AllUsers[i]->isSameID(t))
			{
				flag = 1;
			}
			else
			{
				i++;
			}

		}
		if (flag == 1)
			return AllUsers[i];
		else
			return nullptr;
	}
	Page* SearchPageByID(char t[])
	{
		int flag = 0;
		int i = 0;

		while (AllPages[i] != 0 && flag == 0)
		{
			if (AllPages[i]->isSameID( t))
			{
				flag = 1;
			}
			else
			{
				i++;
			}

		}
		if (flag == 1)
			return AllPages[i];
		else
			return nullptr;
	}
	Object* SearchObjectByID(char t[])
	{

		Object* o = SearchPageByID(t);
		if (!o)
			o = SearchUserByID(t);
		if (!o)
			return nullptr;
		else
			return o;
	}
	Post* SearchPostByID(char t[])
	{
		int flag = 0;
		int i = 0;

		while (AllPosts[i] != 0 && flag == 0)
		{
			if (AllPosts[i]->isSameId(t))
			{
					flag = 1;
			}
			else
			{
				i++;
			}

		}
		if (flag == 1)
			return AllPosts[i];
		else
			return nullptr;
	}
	void LinkUsersandPages(const char* fname)
	{
		ifstream fin(fname);
		char* delimiter = GetStringFromBuffer("-1");
		char id[10];
		fin >> id;
		while (StringCompare(delimiter, id) == false)
		{
			User* temp = SearchUserByID(id);
			char friendid[10];
			fin >> friendid;
			while (StringCompare(delimiter, friendid) == false)
			{
				User* userfriend = SearchUserByID(friendid);
				temp->AddFriend(userfriend);
				fin >> friendid;
			}
			char pageid[10];
			fin >> pageid;
			while (StringCompare(delimiter, pageid) == false)
			{
				Page* userlikedpage = SearchPageByID(pageid);
				temp->LikePage(userlikedpage);
				fin >> pageid;
			}
			fin >> id;
		}
		fin.close();
	}
	void LoadAllUsers(const char* fname)
	{
		ifstream fin(fname);
		int total;
		fin >> total;
		AllUsers = new User * [total + 1] {0};
		for (int i = 0;i < total;i++)
		{
			AllUsers[i] = new User;
			AllUsers[i]->ReadDataFromFile(fin);
		}
		AllUsers[total] = 0;
		fin.close();
	}
	void LoadAllPages(const char* fname)
	{
		ifstream fin(fname);
		int total;
		fin >> total;
		AllPages = new Page * [total + 1]{0};
		for (int i = 0;i < total;i++)
		{
			AllPages[i] = new Page;
			AllPages[i]->ReadDataFromFile(fin);
		}
		AllPages[total] = 0;
		fin.close();
	}
	void LoadAllPosts(const char* fname)
	{
		ifstream fin(fname);
		int total;
		fin >> total;
		AllPosts = new Post * [total+1] {0};
		for (int i = 0;i < total;i++)
		{
			AllPosts[i] = new Post;
			AllPosts[i]->ReadDataFromFile(fin);
			char temp[10];
			fin >> temp;
			Object* ptr = SearchObjectByID(temp);
			AllPosts[i]->SetSharedBy(ptr);
			ptr->AddToTimeline(AllPosts[i]);
			ptr = 0;
			fin >> temp;
			char* delimiter = GetStringFromBuffer("-1");
			while (StringCompare(delimiter, temp) == false)
			{
				ptr = SearchObjectByID(temp);
				AllPosts[i]->SetLikedBy(ptr);
				ptr = 0;
				fin >> temp;
			}
		}
		AllPosts[total] = 0;
		fin.close();
	}
	void LoadAllComments(const char* fname)
	{
		ifstream fin(fname);
		int total;
		fin >> total;
		for (int i = 0;i < total;i++)
		{
			char id[10];
			fin >> id;
			fin.ignore();
			char postid[10];
			fin >> postid;
			fin.ignore();
			char objid[10];
			fin >> objid;
			Object* optr = SearchObjectByID(objid);
			fin.ignore();
			char text[100];
			fin.getline(text, 100);
			Comment* ptr = new Comment(GetStringFromBuffer(id), optr, GetStringFromBuffer(text));
			Post* postptr = SearchPostByID(postid);
			postptr->AddComment(ptr);
		}
		fin.close();
	}
	void LoadAllContent(const char* fname)
	{
		ifstream fin(fname);
		int total;
		fin >> total;
		for (int i = 0;i < total;i++)
		{
			char temp[50];
			fin >> temp;
			Post* postptr = SearchPostByID(temp);
			PostContent* ptr = new Activity;
			ptr->ReadFromFile(fin);
			postptr->AddContent(ptr);
		}
		fin.close();
	}
	void LoadData()
	{
		LoadAllUsers("users.txt");
		LoadAllPages("pages.txt");
		LinkUsersandPages("usersandpages.txt");
		LoadAllPosts("posts.txt");
		LoadAllComments("comments.txt");
		LoadAllContent("postcontent.txt");
	}
	void LikePost(Post*& p, User*& u)
	{

		if (p)
		{
			if (p->isPostable())
			{
				Object* o = u;
				p->SetLikedBy(o);
				cout << "DONE SUCCESSFULLY\n";
			}
			else
			{
				cout << "NO POST IS PRESENT WITH THIS ID TO BE LIKED" << endl;
			}
		}
		else
		{
			cout << "NO POST IS PRESENT WITH THIS ID TO BE LIKED" << endl;
		}
	}
	void ViewLikedList(Post*&p)
	{
		
		if (p)
		{
			if (p->isPostable())
			{
				cout << "Post Liked By:\n";
				p->PrintLikes();
			}
			else
			{
				cout << "NO POST IS PRESENT WITH THIS ID TO BE LIKED" << endl;
			}
		}
		else
		{
			cout << "NO POST IS PRESENT WITH THIS ID TO BE LIKED" << endl;
		}
	}
	bool IsPostPresent(char* p)
	{
		int i = 0, flag = 0;
		while (AllPosts[i] != 0 && flag == 0)
		{
			if (AllPosts[i]->isSameId(p))
			{
				if (AllPosts[i]->isPostable())
				{
					flag = 1;
				}
			}
			i++;
		}
		if (flag == 1)
		{
			return true;
		}
		return false;
	}
	void PostComment(char* postid,char* t,User*&u)
	{
		cout << "COMMAND: PostComment(" << postid << "," << t << ")\n";
		cout << "--------------------------------------------------------------------\n";
		if (IsPostPresent(postid))
		{
			Post* ptr = SearchPostByID(postid);
			Object* o = u;
			char* text = GetStringFromBuffer(t);
			Comment* c = new Comment(o, text);
			ptr->AddComment(c);
			cout << "DONE SUCCESSFULLY\n";
		}
		else
		{
			cout << "THIS POST IS NOT PRESENT"<<endl;
		}
	}
	void ViewPost(Post*&ptr)
	{
		
		if (ptr)
		{
			if (ptr->isPostable())
			{
				cout << "~~~~~~~~~~~~~~~~~";
				ptr->PrintID();
				cout<< "~~~~~~~~~~~~~~~~~~~~" << endl;
				ptr->PrintListView();
				cout << endl << endl;
			}
			else
			{
				cout << "THIS POST IS NOT PRESENT\n";
			}
		}
		else
		{
			cout << "THIS POST IS NOT PRESENT\n";
		}
	}
	void ShareMemory(User*&u,const char*t,char*p)
	{
		int i = 0,flag=0;
		Post* postptr = SearchPostByID(p);
		while (AllPosts[i]!=0 && flag==0)
		{
			if (AllPosts[i] == postptr)
			{
					if (postptr->isPostable() && postptr->isSharedBy(u) && postptr->yeardifference()!=0)
					{
						Post* ptr = new Memory(t, postptr, u);
						u->AddToTimeline(ptr);
						flag = 1;
					}
					else if (postptr->isSharedBy(u) == false)
					{
						cout << p << " is not shared by ";
						u->PrintName();
						cout << endl;
						flag = 1;
					}
					else
					{
						cout << "This post is not a valid memory\n";
					}

			}

			i++;
		}
	}
	void SeeYourMemories(User* user)
	{
		int i = 0,flag=0;
		cout << "We hope you enjoy looking back and sharing your memories on Facebook, from the most recent to those long ago." << endl << endl;
		while (AllPosts[i])
		{
				if (AllPosts[i]->isSharedBy(user))
				{
					if (AllPosts[i]->yeardifference() != 0)
					{
						if (AllPosts[i]->isPostable())
						{
							cout << "On this day" << endl;
							cout << AllPosts[i]->yeardifference() << " years ago\n";
							AllPosts[i]->PrintListView();
							flag = 1;
						}
					}
				}
			i++;
		}
		if (flag == 0)
		{
			cout << "NO MEMORIES ON THIS DAY"<<endl;
		}

	}
	void Run()
	{
		int day = 17, month = 4, year = 2024;
		Date::setCurrentDate(day, month, year);
		cout << "--------------------------------------------------------------------\n";
		cout << "System Date:\t";
		Date::PrintCurrentDate();
		cout << "--------------------------------------------------------------------\n";
		char userid[10] = "u7";
			cout << "COMMAND: Set Current User " << userid << endl;
			cout << "--------------------------------------------------------------------\n";
			User* currentuser = SearchUserByID(userid);
			if (currentuser)
			{
				currentuser->PrintName();
				cout << " is set with ID: ";
				currentuser->PrintID();
				cout << endl;
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewFriendsList()\n";
				cout << "--------------------------------------------------------------------\n";
				currentuser->ViewFriendsList();

				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewLikedPages()\n";
				cout << "--------------------------------------------------------------------\n";
				currentuser->ViewLikedPages();
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewTimeline()\n";
				cout << "--------------------------------------------------------------------\n";
				currentuser->ViewTimeline();
				cout << endl << endl;


				char postid[10] = "post5";
				Post* post = SearchPostByID(postid);
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewLikedList(" << postid << ")\n";
				cout << "--------------------------------------------------------------------\n";
				ViewLikedList(post);
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: LikedPost(" << postid << ")\n";
				cout << "--------------------------------------------------------------------\n";
				LikePost(post, currentuser);
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewLikedList(" << postid << ")\n";
				cout << "--------------------------------------------------------------------\n";
				ViewLikedList(post);


				char pageid[10] = "p1";
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewPage(" << pageid << ")\n";
				cout << "--------------------------------------------------------------------\n";
				Page* page = SearchPageByID(pageid);
				if (page)
				{
					page->ViewTimeline();
				}
				else
				{
					cout << "NO PAGE IS PRESENT WITH THIS ID\n";
				}
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewHome\n";
				cout << "--------------------------------------------------------------------\n";
				currentuser->ViewHome();


				strcpy_s(postid, "post4");
				post = 0;
				post = SearchPostByID(postid);
				char text[100] = "Good Luck For Result!";
				cout << "--------------------------------------------------------------------\n";
				PostComment(postid, text, currentuser);
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewPost(" << postid << ")\n";
				cout << "--------------------------------------------------------------------\n";
				ViewPost(post);
				cout << "--------------------------------------------------------------------\n";
				strcpy_s(postid, "post8");
				strcpy_s(text, "Thanks for the wishes!");
				post = 0;
				post = SearchPostByID(postid);
				cout << "--------------------------------------------------------------------\n";
				PostComment(postid, text, currentuser);
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: ViewPost(" << postid << ")\n";
				cout << "--------------------------------------------------------------------\n";
				ViewPost(post);
				cout << "--------------------------------------------------------------------\n";
				cout << "COMMAND: SeeYourmemories()\n";
				cout << "--------------------------------------------------------------------\n\n";
				SeeYourMemories(currentuser);
				cout << "--------------------------------------------------------------------\n";
				strcpy_s(postid, "post10");
				strcpy_s(text, "Never thought I will be a specialist in this field");
				cout << "COMMAND: ShareMemory(" << postid << ",\"" << text << "\")\n";
				cout << "COMMAND: ViewTimeline()\n";
				cout << "--------------------------------------------------------------------\n";
				ShareMemory(currentuser, text, postid);
				currentuser->ViewTimeline();
				cout << "--------------------------------------------------------------------\n";
			}
			else
			{
				cout << "NO USER FOUND WITH THIS ID\n";
			}
			currentuser = 0;
		strcpy_s(userid, "u11");
		currentuser = SearchUserByID(userid);
		if (currentuser)
		{
			cout << "COMMAND: Set Current User " << userid << endl;
			cout << "--------------------------------------------------------------------\n";
			currentuser->ViewTimeline();
		}
		else
		{
			cout << "NO USER FOUND WITH THIS ID\n";
		}

	}
	~Controller()
	{
		int i = 0;
		if (AllUsers)
		{
			while (AllUsers[i] != 0)
			{
				delete AllUsers[i];
				i++;
			}

			delete[]AllUsers;
		}
		i = 0;
		if (AllPages)
		{
			while (AllPages[i] != 0)
			{
				delete AllPages[i];
				i++;
			}
			delete[]AllPages;
		}
		i = 0;
		if (AllPosts)
		{
			delete[]AllPosts;
		}
	}
};


void main()
{
	Controller mainApp;
	mainApp.LoadData();
	mainApp.Run();
	
}
